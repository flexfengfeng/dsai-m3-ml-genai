# L10 Pre-class · ≈ 75 minutes

> **Goal:** Walk into the final lesson having experienced GenAI hands-on, having an intuition for attention, and having one specific question you'd like answered.

## Step 1 — Watch the attention video (≈ 25 min)

3Blue1Brown, *"Attention in transformers, step by step"*  
https://www.youtube.com/watch?v=eMlx5fFNoYc

Focus questions to hold in mind:
- What is a "query" vector? A "key" vector? A "value" vector?
- Why does the model need to compute similarity between EVERY pair of tokens?
- What's "multi-head" attention? Why have several heads?

Don't try to memorise the maths — pay attention to the *shape* of the operation. The maths is one matrix multiplication and one softmax.

## Step 2 — Optional read: how transformers actually scale (≈ 10 min)

Andrej Karpathy, *"State of GPT"* — slides or YouTube  
(Pick any 10-minute section. The history of how we got from GPT-1 to GPT-4 in 6 years.)

## Step 3 — Run [notebooks/01_monday_morning.ipynb](notebooks/01_monday_morning.ipynb) (≈ 35 min)

Sarah's Friday inbox has another problem to solve. Before she dives in, she explores what pretrained transformers can already do out of the box — sentiment, named-entity recognition, summarisation, translation — all in single Python lines.

The notebook walks through:
1. Loading three pretrained pipelines (sentiment, NER, summarisation) — each is one line
2. Running them on real-style product reviews and announcements
3. Watching the models do non-trivial NLP work without you writing any model code
4. Reading the small print: pipelines are convenient, but they're loading 100M-400M parameter transformers under the hood

Expected outcome: you'll finish convinced that *something* powerful has happened in NLP, and curious about how it works.

## Step 4 — Three thought-questions (≈ 10 min)

Write your answers in a notebook cell or scratch pad. We'll discuss in class.

1. The sentiment pipeline labelled a review as POSITIVE with confidence 0.99. **What was it actually trained to predict?** Take a guess — and what would the training data have looked like?
2. ChatGPT, Claude, and Llama are all *transformer-based*. So is `all-MiniLM-L6-v2` from L09. So is the sentiment classifier. **What's different about them?** Is it the architecture? The size? The training data? The objective? All four?
3. If you had to add **one** GenAI feature to a real product you use, what would it be? Write one sentence — we'll see in class whether your idea is well-suited to a transformer, or if a simpler tool would do better.

---

**You are ready for class when:** you've finished the notebook, you can explain (in your own words) what "attention" approximately does, and you have one specific GenAI use-case in mind that you'd like to know whether is feasible.
