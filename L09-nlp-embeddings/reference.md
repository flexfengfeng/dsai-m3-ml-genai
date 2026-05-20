# L09 Reference · Glossary

| Term | Definition |
|------|------------|
| **Token**             | A unit of text fed to a model — could be a whole word, a subword piece, a character, or a special symbol. |
| **Tokeniser**         | The component that splits raw text into tokens. Modern ones use BPE / WordPiece / SentencePiece (subword). |
| **Subword tokenisation** | Splitting words into smaller pieces ("running" → "run" + "##ning") so the model has a fixed vocabulary but can still handle novel words. |
| **Vocabulary**        | The fixed set of tokens a model knows. Typical size: 30K-100K. |
| **One-hot encoding**  | Representing a token as a vector of zeros with a single 1 at its vocab index. Sparse, large, captures identity not meaning. |
| **Embedding**         | A dense, low-dimensional (50-1000 D) vector representation of a token or sentence, learned from data. Similar meaning → similar vector. |
| **Word embedding**    | An embedding for a single word/token (Word2Vec, GloVe). One vector per word. |
| **Sentence embedding**| An embedding for a whole sentence/paragraph. Captures the meaning of the entire string. |
| **Distributional hypothesis** | Firth's 1957 idea: words appearing in similar contexts have similar meanings. The basis for all learned embeddings. |
| **Word2Vec**          | A 2013 family of algorithms (Mikolov et al.) for learning word embeddings from context windows. |
| **GloVe**             | Stanford's 2014 word-embedding method based on global word co-occurrence statistics. |
| **BERT**              | Google's 2018 transformer that produces *contextual* word embeddings — the embedding for "bank" depends on the surrounding sentence. |
| **Sentence-Transformer** | A model fine-tuned to produce sentence-level embeddings (Reimers & Gurevych, 2019). `all-MiniLM-L6-v2` is a small, fast example. |
| **Cosine similarity** | A measure of how similar two vectors are, based on the angle between them: `dot(a, b) / (||a|| * ||b||)`. Range: [-1, 1]. |
| **TF-IDF**            | Term-frequency × inverse-document-frequency. Classical text-retrieval scoring. Keyword-based. |
| **Bag-of-words (BoW)** | Representing a document as a sparse count vector over the vocabulary. Loses word order; loses meaning. |
| **Semantic search**   | Retrieval by meaning rather than keyword match. Built on sentence embeddings + cosine similarity. |
| **Top-K retrieval**   | Returning the K most similar items to a query, ranked by similarity score. |
| **Hybrid retrieval**  | Combining lexical (TF-IDF / BM25) and semantic (embedding) scoring. Standard in modern production search. |
| **Re-ranking**        | A second pass that re-orders top-K results using additional signals (popularity, recency, price, user history). |
| **Vector database**   | A specialised store optimised for fast nearest-neighbour search over millions of embeddings (FAISS, Pinecone, Weaviate, etc.). |
| **Hugging Face Hub**  | The standard public registry for pretrained NLP models. `model = SentenceTransformer("all-MiniLM-L6-v2")` downloads from here. |

---

## Further reading & watching

Optional resources that go deeper than what we cover in class. Pick what interests you; nothing here is required.

### Reading

- **[The Illustrated Word2Vec](https://jalammar.github.io/illustrated-word2vec/)** — Jay Alammar — *Read the first half (up to and including "Word2Vec Training"). The clearest explanation of *why* embeddings work.*
- [Summary of the Tokenisers](https://huggingface.co/docs/transformers/tokenizer_summary) — Hugging Face docs — *Skim the BPE / WordPiece / SentencePiece sections to see how modern models split text*
