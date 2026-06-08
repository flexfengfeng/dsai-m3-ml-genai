# Simple Learning Guide: ML for Adult Learners (No Prior Experience)

## How the Course Works
- **Story‑driven:** Follow Sarah Chen at NorthStar Retail; each lesson solves her next business challenge.
- **Hands‑first:** Run code and see results in the first 20 minutes (pre‑class notebook).
- **Reflect → Learn:** After seeing what ML can do, we cover the intuition and tools.
- **Checklists:** Every lesson gives a 3‑step practical checklist you can use immediately at work.
- **Weekly effort:** ~25 min pre‑class + 3–5 h notebooks + assignment (self‑study).

## Weekly Flow (Example)
1. **Pre‑class:** Run the indicated notebook (e.g., `01_monday_morning.ipynb`) and answer 3 short reflection questions.
2. **In‑class notebooks:** Work through the notebooks in order (shown in `lesson.md` → “How Lxx is taught”).
3. **Lesson reference:** Read `lesson.md` for takeaways, checklists, and review questions.
4. **Check your understanding:** Try the questions before looking at the sample answers.
5. **Assignment:** Apply the week’s ideas to a new domain (banking, recipes, etc.).
6. **Optional extensions:** Explore deeper if interested (no pressure).

## Core Mindsets You’ll Build
- **Preprocessing discipline:** Clean data *before* modeling; split first, then fit transforms on training only.
- **Honest evaluation:** Always check shape, report uncertainty (CI/ranges), and use a ground‑truth set when possible.
- **Business‑first decisions:** Choose thresholds, models, or techniques based on operational capacity, error costs, and stakeholder needs—not just accuracy.
- **Transfer learning mindset:** Start with pretrained models (images, text) unless you have lots of data.
- **System thinking:** Evaluate parts separately (e.g., retrieval quality before generation in RAG).

## Typical Lesson Checklist (examples)
- **L01 – Is ML the right tool?**  
  1. Rules hard to write by hand?  
  2. Have labeled data (or can get it)?  
  3. Is “right most of the time” good enough?  
- **L02 – Honestly reporting a number**  
  1. What’s the shape of the data? (skew → use median or segment)  
  2. What range is the true value likely in? (add confidence interval)  
  3. Could this difference be random? (report p‑value + effect size)  
- **L03 – Threshold choice**  
  1. What’s the team’s capacity to act?  
  2. What’s the cost of each error type?  
  3. Did you pick the threshold from business inputs, not the test set?  
- **L04 – Model‑shipping**  
  1. Is the accuracy gap big enough to matter operationally?  
  2. What does the team have to maintain? (dependencies, stability)  
  3. How explainable does this need to be?  
- **L05 – Technique choice**  
  1. What’s the deliverable? (picture → PCA, label per row → K‑Means, watch list → Isolation Forest)  
  2. Is preprocessing the same as for supervised models? (median‑impute, standard‑scale, one‑hot encode)  
  3. Can you defend the choice with a metric *and* a sentence?  
- **L06 – Forecast honestly**  
  1. Did you decompose first? (look at trend + seasonality + residual)  
  2. Did a trivial baseline lose? (compare to Naive/Seasonal Naive)  
  3. Was evaluation time‑respecting? (use TimeSeriesSplit, never random k‑fold)  
- **L07 – Training run**  
  1. Did it converge? (train loss flat, not still dropping)  
  2. Is it overfitting? (val loss rising while train loss falls)  
  3. Is the learning rate right? (smooth curve, not jagged or flat)  
- **L08 – Transfer learning**  
  1. How many labeled images per class? (<1K → start pretrained)  
  2. How close is your domain to ImageNet? (close → head‑only; far → fine‑tune last block)  
  3. Did you match the pretrained model’s preprocessing contract? (ImageNet mean/std, RGB, proper size)  
- **L09 – Validate semantic search**  
  1. Do you have a labeled ground‑truth query set?  
  2. Did you measure precision@k *and* inspect the failures?  
  3. Have you planned for exact‑match (TF‑IDF) and stale‑embedding (re‑encoding) cases?  
- **L10 – Design RAG**  
  1. Is retrieval pulling the right documents? (evaluate retrieval first)  
  2. Is the prompt forcing the model to stay grounded? (explicit “use only these” + safety check)  
  3. Do you have an evaluation set, not just a demo? (held‑out query → answer pairs)

## Study Tips
- **Explain it out loud** after each notebook: “What did we do? Why? What would I tell my manager?”
- **Connect to your work:** After each lesson, write down one place you could apply the idea.
- **Focus on the checklist**, not memorizing formulas.
- **Embrace productive struggle:** If stuck, spend 10 min trying to re‑explain using only the notebook/lesson before searching.
- **Use optional extensions** only if a topic fascinates you; otherwise skip and return later.
- **Celebrate small wins:** “I built my first model,” “I explained a confidence interval,” etc.

## Common Pitfalls to Avoid
- Treating ML as a magic black box → always start with “Is ML the right tool?”
- Chasing accuracy without context → use threshold‑ and model‑shipping checklists.
- Skipping preprocessing → remember 60‑70% of effort is here; always split before preprocessing.
- Over‑trusting a single number → add confidence intervals and check distribution.
- Using wrong evaluation for time series → use TimeSeriesSplit, never random k‑fold.
- Assuming neural networks are always better → on tabular data, start with linear/GB baselines.
- Training deep models from scratch on tiny data → start pretrained (<1K images/class).
- Trusting cosine as a percentage → it’s a ranking signal, not a %.
- Believing an LLM knows your data → ground it with retrieval (RAG).
- Neglecting to inspect failures → always look at the actual top‑k for queries you got wrong.

## Getting Help
- **Lesson markdown (`lesson.md`)** – quick reference for takeaways and checklists.
- **Reference (`reference.md`)** – glossary, further reading, character/context.
- **Notebook markdown cells** – read the text above/below code for intent.
- **Assignment notebooks** – see how ideas apply to a new domain; try before peeking at solutions.
- **Optional extensions** – deeper dives if interested.
- **Study group / peers** – discuss weekly; teaching others solidifies learning.
- **Online docs** – scikit‑learn, PyTorch, sentence‑transformer links are in `reference.md`.
- **Instructor / forum** – bring specific conceptual questions that don’t clear after self‑help.

## Final Goal
By the end you’ll be able to:
1. Look at a business problem and ask: “Is ML the right tool? What would success look like?”
2. Build a trustworthy model pipeline (preprocess → train/validate → evaluate honestly).
3. Interpret results in context (distributions, uncertainty, error costs, stakeholder needs).
4. Communicate findings honestly to technical and non‑technical audiences.
5. Learn new ML techniques quickly because you understand the underlying workflow and mindset.

You’ll have seen Sarah grow from a new analyst with a USB drive of reviews to the person who builds a production‑ready, retrieval‑augmented generation system for her company’s catalogue. Your journey mirrors that growth—one honest, practical step at a time.

**Welcome to the course. Let’s get started.**