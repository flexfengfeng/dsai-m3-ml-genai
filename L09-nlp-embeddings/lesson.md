# L09 — Lesson · NLP, Embeddings & Semantic Search

> *Sarah's catalogue auto-tagger shipped in L08. Now the customer-experience team is unhappy with search — a shopper typed "blue summer dress" and got nothing. The catalogue uses words like "frock", "sundress", "gown". Keyword search fails on synonyms. We need a model that understands meaning.*

## Part 1 — Why keyword search breaks

### 1.1 The toy problem

A customer types: **"blue summer dress"**.  
The catalogue has 10 dresses. The most relevant one is described as:

> *"Lightweight floral frock perfect for warm summer days. Tea-length, pastel print, breathable cotton."*

There are no overlapping content words between query and description (besides "summer"). Keyword search returns 1 weak match instead of 10 strong ones.

### 1.2 What's actually broken

Keyword search treats every word as a unique token, independent of every other word. It has no notion that:

- *frock*, *sundress*, *gown*, *dress* all refer to the same garment type
- *floral*, *pastel*, *bright* are co-related descriptors
- *lightweight*, *breathable*, *cotton* together imply *summer-appropriate*

Bandaids (stemming, stop-words, lemmatisation) help marginally but don't solve the synonym problem.

### 1.3 What we need

A representation where words and sentences with **similar meanings** end up **close together** mathematically. That representation is **embeddings**.

## Part 2 — Words as vectors

### 2.1 One-hot encoding (the naive baseline)

Take all unique words in the vocabulary. Assign each a unique index. Encode a word as a vector of all zeros with a 1 at its index.

```
vocab = ["cat", "dog", "frock", "dress", ...]   # 50,000 words
"frock" → [0, 0, 1, 0, ..., 0]                  # 50,000-dim
"dress" → [0, 0, 0, 1, ..., 0]
```

Problems:
- Vectors are huge (vocabulary size).
- Every pair of distinct words is **equidistant**. Cosine similarity between "frock" and "dress" is exactly 0 — same as between "frock" and "spaceship".

One-hot encoding captures identity, not meaning.

### 2.2 The embedding idea

Replace each one-hot vector with a much smaller, dense vector of real numbers (say, 300 dimensions). The numbers are **learned** so that semantically similar words have similar vectors.

```
"frock"   → [0.21, -0.43, 0.88, ..., 0.12]   # 300-dim
"dress"   → [0.19, -0.41, 0.85, ..., 0.15]   # very close to "frock"
"banana"  → [-0.7, 0.2, -0.5, ..., 0.9]      # very different
```

The vectors don't have intrinsic meanings (dimension 17 is not "is feminine"). What matters is the **relationships**:

```
cosine(frock, dress)  ≈ 0.91   (synonyms — high)
cosine(frock, banana) ≈ 0.08   (unrelated — low)
```

This is the entire promise of word embeddings: turn discrete tokens into continuous vectors where distance means meaning.

### 2.3 Where do the numbers come from?

Embeddings are *learned* from text. The classic recipe (Word2Vec, 2013):

1. Take a large text corpus (e.g. all of Wikipedia).
2. For every word, look at its **context** — the words that appear nearby.
3. Train a small neural network to predict context from the word (or vice versa).
4. The network's hidden-layer weights become the embeddings.

Why this works: *words appearing in similar contexts tend to mean similar things*. This is the **distributional hypothesis** (Firth, 1957: "you shall know a word by the company it keeps").

Modern models (BERT, MiniLM, etc.) go further — they learn embeddings that depend on context. The word "bank" gets one embedding in "river bank" and a different one in "bank account". But the underlying principle is the same.

## Part 3 — Sentence embeddings

### 3.1 From words to sentences

A sentence embedding represents the **meaning of a whole sentence** as a single vector. You can compute one in two ways:

- **Average the word embeddings** in the sentence (cheap, decent baseline)
- **Run the sentence through a model trained specifically for sentence embeddings** (better)

The second approach is what `sentence-transformers` does. The library wraps models like `all-MiniLM-L6-v2` — a small transformer that takes a sentence in and returns a single 384-dimensional vector capturing its meaning.

### 3.2 Practical usage

```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer("all-MiniLM-L6-v2")
embeddings = model.encode([
    "blue summer dress",
    "lightweight floral frock perfect for warm summer days",
    "industrial cargo trousers",
])
# embeddings.shape == (3, 384)
```

Three sentences in, a 3×384 matrix out. To find the most similar sentence to the first, compute cosine similarity of row 0 against rows 1 and 2:

```python
from sklearn.metrics.pairwise import cosine_similarity
sims = cosine_similarity(embeddings[:1], embeddings[1:])
# sims ≈ [[0.62, 0.14]]
# → sentence 1 (frock) is much closer to the query than sentence 2 (trousers)
```

Even though the query and the frock description **share zero content words besides "summer"**, the model assigns them high similarity. That's the semantic search promise delivered.

### 3.3 What the model actually learnt

`all-MiniLM-L6-v2` was trained on over a billion sentence pairs labelled as "semantically similar" or "not". It learned to map sentences into a space where:

- Paraphrases sit near each other
- Sentences about the same topic cluster together
- Sentences with opposite meanings sit far apart

You're not training anything yourself. You're loading a model that already encodes general English meaning, and pointing it at your data.

## Part 4 — Building a search engine

### 4.1 The semantic search recipe

This is the entire pipeline:

```
1. Embed every product description once (offline, cached)
   catalogue_embeddings: shape (N_products, 384)

2. At query time:
   a. Embed the user's query — one new (384,) vector
   b. Compute cosine similarity of query vector against all catalogue vectors
   c. Return the top-K highest similarities

3. (Optional) Re-rank top-K by additional signals (price, popularity, recency)
```

That's it. No training, no fine-tuning, no labels. A pretrained model + a couple of lines of NumPy.

### 4.2 Comparing to TF-IDF

TF-IDF (term-frequency × inverse-document-frequency) is the classical text-retrieval baseline. It's keyword search with cleverer weighting — but still keyword search. On the catalogue:

| Approach            | Strength | Weakness |
|---------------------|----------|----------|
| TF-IDF              | Fast, no model, robust to typos | Fails on synonyms (frock ≠ dress) |
| Sentence embeddings | Understands meaning, handles synonyms | Slower, needs a model, less precise on rare exact terms |

For real production search you use **both** — combine TF-IDF for exact matches with embeddings for semantic recall. We'll keep it to embeddings only in this lesson.

## Part 5 — Sarah's recommendation to Marcus

> *"For the catalogue search, swap the keyword-only matcher with a pretrained sentence-transformer (all-MiniLM-L6-v2). Pre-compute embeddings for all 12,000 products once — takes ~2 minutes on CPU, ~30 seconds on the GPU box. At query time, embed the user's input and find top-20 by cosine similarity. Re-rank that 20 by popularity and price."*
>
> *"Expected impact: 'meaningless-result' rate drops from ~25% (keyword only) to ~5% (semantic + popularity rerank). Customer search abandonment should drop too — we'll measure with an A/B test."*

She flags two production points:

1. **Re-embed when descriptions change.** Embeddings are a function of the description text — if merchandisers rewrite a description, that product's embedding goes stale until you re-embed.
2. **Keep TF-IDF as a fallback.** For exact searches like product codes ("P0042") or brand names, exact match still wins. Send those queries to TF-IDF, send the rest to embeddings.

This is the standard pattern in industrial search engines: hybrid retrieval. Embeddings are the new player, not the only player.

## Part 6 — Bridge to L10

We've now treated the embedding model as a black box: we put text in, we get a vector out. But how does it *work*? How does it know that "frock" and "dress" are similar without ever being told?

The answer is **transformers and attention**, the architecture invented in 2017 and behind every modern NLP model. In L10 we open the black box: we'll see how attention mechanisms let a model decide which words in a sentence to focus on, how a transformer block stacks attention with feed-forward layers, and how the same architecture scales from `all-MiniLM-L6-v2` (22M params) to GPT-4 (estimated 1T+ params).

By the end of L10 you'll understand the architecture that runs ChatGPT.

---

## Recap — three things to remember

1. **Embeddings turn discrete tokens into dense vectors where distance = meaning.** Same garment, different words → similar vectors.
2. **Pretrained sentence-transformers are the default starting point.** Don't train from scratch unless you have a specific reason.
3. **Semantic search is two functions:** `embed_corpus_once()` + `cosine_similarity(query_emb, corpus_embs)`. The infrastructure is trivial; the model does the heavy lifting.
