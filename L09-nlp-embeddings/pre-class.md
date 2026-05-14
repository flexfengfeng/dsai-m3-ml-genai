# L09 Pre-class · ≈ 75 minutes

> **Goal:** Walk into class already convinced that keyword search is broken, and ready to learn what's going to replace it.

## Step 1 — Watch the embeddings primer (≈ 20 min)

Jay Alammar, *"The Illustrated Word2Vec"* — read the first half (up to and including "Word2Vec Training").
https://jalammar.github.io/illustrated-word2vec/

Focus questions:
- What does it mean for a word to be a *vector*?
- Why do related words end up close together in vector space?
- What's the difference between "skip-gram" and "CBOW" training? (Don't worry about the maths.)

## Step 2 — Optional read: tokenisation (≈ 10 min)

Hugging Face docs, *"Summary of the tokenisers"*  
https://huggingface.co/docs/transformers/tokenizer_summary

Skim the section on **BPE / WordPiece / SentencePiece**. You don't need to memorise; just know that modern models split words into *subword tokens*, not whole words.

## Step 3 — Run [notebooks/01_monday_morning.ipynb](notebooks/01_monday_morning.ipynb) (≈ 35 min)

This is the failure-mode hook. Sarah is at her laptop trying to understand a customer complaint about search.

The notebook walks through:
1. Loading NorthStar's product catalogue (76 products, real-feeling descriptions)
2. Implementing **keyword search** the obvious way: lowercase, split, look for matches
3. Running the user's actual query — *"blue summer dress"* — and watching it return very few useful results
4. Diagnosing **why**: the descriptions use synonyms (frock, sundress, gown), and a query that uses different words to mean the same thing finds nothing
5. A second attempt with stemming and stop-words — slightly better but still bad

Expected outcome: you should finish the notebook with a clear, lived sense that *"meaning" is not "shared words"*. Tomorrow we'll meet the tool that fixes it.

## Step 4 — Three thought-questions (≈ 10 min)

Write your answers in a notebook cell or scratch pad. We'll discuss in class.

1. The customer typed *"blue summer dress"*. List **five** different ways a product description could legitimately describe the same kind of dress without using ANY of those three words.
2. Try thinking like a model: if every word were a separate dimension of a vector, the vector for *"blue summer dress"* and *"floral pastel frock"* would have **zero overlap** even though they mean similar things. What's the missing piece?
3. Search a real e-commerce site for *"comfortable office shoes"*. Does the search engine seem to know what you mean, or is it just keyword-matching? How can you tell?

---

**You are ready for class when:** you've finished the notebook, you can articulate exactly *what's broken* about keyword search, and you have one or two guesses about what could fix it.
