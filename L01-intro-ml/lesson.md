# Lesson — L01 Introduction to Machine Learning

> **Chapter 1 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · January 2023.*
> Early in her second week, Aisha from Customer Service hands her a USB drive with 10,000 reviews. Priya, her manager, wants a sentiment breakdown by Friday.

This document is a **short reference** — the lesson itself is taught in the notebooks. Read it for orientation before class, then come back to it for the takeaways, the ML-fit checklist, the review questions, and the course map.

---

## How L01 is taught

| Stage | Where to go |
|---|---|
| **Pre-class** | `pre-class.md` + `notebooks/01_monday_morning.ipynb` |
| **In-class — Part 1: What is ML?** | `notebooks/02_what_is_ml.ipynb` |
| **In-class — Part 2: Three categories** | `notebooks/03_three_categories.ipynb` |
| **In-class — Part 3: The ML workflow** | `notebooks/04_ml_workflow.ipynb` |
| **Self-study** | `notebooks/assignment.ipynb` (Sarah on secondment to Lakeside Bank) + `notebooks/optional_extensions.ipynb` |
| **Reference & review** | This document |

The notebooks are the spine. Run them in order. Come back here for the consolidated takeaways and the review questions.

---

## Overview

Sarah's job this week is to classify 10,000 customer reviews — too many to read by hand, and the rules are too fuzzy to write by hand. That makes it an ML problem. By Friday she will use a pre-trained sentiment model to label every review, recognise that her task is **supervised** learning, and place her work inside the **7-step workflow** that every real ML project follows. L01 exists so the rest of M3 has a shared mental model: every method we will study later — regression, trees, neural nets, GenAI — is a variation on "show the computer examples and let it learn the rules."

---

## Key takeaways

1. **ML is "show, don't tell" programming.** Instead of writing rules, you give the computer labelled examples and let it learn the rules itself.
2. **Three categories — recognise them, but for now we only build supervised.** L01–L04 and L08–L10 are supervised; L05 is unsupervised; reinforcement is mentioned, not built.
3. **A real ML project is 60–70% framing, collection, and cleaning.** Training and evaluation are 10–20%. Deploy and monitor are another 20%. Beginners expect the opposite split.
4. **Framing is the highest-leverage step.** A perfect model solving the wrong problem is worth zero. Always pin down: what's the question, who acts on the answer, what counts as success, and what's the cost of being wrong each way?
5. **Evaluation must use unseen data.** A model that scores 99% on training data can score 60% on data it has never seen. Always hold out a test set.
6. **Models decay; monitor and retrain.** "Train once and forget" does not work — the world moves, the data shifts.
7. **The unanswered question is L02.** Sarah's model says most reviews are positive — *but how sure are we?* That is L02 (Probability & Statistics for ML).

---

## Is ML the right tool? — a checklist

Use ML when **all three** are true:

1. **The rules are hard to write by hand.** (Not a formula, not a policy you can encode.)
2. **You have data with examples** of inputs and correct outputs — or you can get it.
3. **Being right most of the time is good enough.** ML is probabilistic, not deterministic.

ML is the **wrong** tool when:

- The rules are obvious and exact (tax calculations, unit conversions).
- The cost of any wrong answer is catastrophic (flight-control software).
- You have no data. ML without data is astrology with more maths.

Half the value of an ML practitioner is knowing when *not* to reach for ML.

---

## Check your understanding

Work through these after you have finished the three Part notebooks. Try each question on your own first — the sample answer follows.

### Part 1 — What is ML?

**Q1 — Rule-based or ML?** NorthStar wants to automatically send a coupon to every customer whose birthday is today. Which approach fits?

> **Sample answer:** Rule-based. "Is today equal to their birthday?" is a one-line `if` statement. No data needed, no uncertainty. An ML model here would add cost and risk for zero benefit.

**Q2 — Fraud detection fit.** A bank has millions of historical transactions labelled fraudulent / not-fraudulent. Which of the three "ML is the right tool" requirements do they satisfy, and which one might be tricky?

> **Sample answer:** They satisfy #1 (fraud patterns are too varied to hand-code) and #2 (lots of labelled data). Requirement #3 is tricky: a false negative (missing real fraud) is expensive but tolerable; a false positive (blocking a legitimate customer) is annoying but recoverable. They need to weigh the cost of each kind of wrong answer — a classic ML design decision we revisit in **L03**.

**Q3 — Training vs inference.** What's the difference between training and inference?

> **Sample answer:** Training is the (often slow, compute-heavy) process of learning the rules from labelled examples. Inference is using the trained model to predict on new data — it is what you do every time the model gives you an answer. In Sarah's case, someone else already trained the sentiment model; she only does inference.

**Q4 — M1 / M2 / M3 test.** Aisha asks Sarah to (a) count how many reviews came in each month of last year, (b) merge the review table with the orders table in the warehouse, and (c) decide whether a new incoming review mentions a shipping complaint. Which module does each task belong to?

> **Sample answer:** (a) M1 — summarising the past. (b) M2 — moving/joining data. (c) M3 — judgement about a new thing, no simple rule. Only (c) needs Machine Learning.

### Part 2 — Three categories

**Q5 — Sarah's review problem.** Which category does Sarah's review-classification problem fit into, given she has a training set of 50,000 past reviews already labelled positive or negative?

> **Sample answer:** Supervised learning. She has features (the review text) and labels (positive / negative). She wants the model to predict labels for new reviews.

**Q6 — Newspaper topics.** A newspaper wants to automatically group its last 10 years of articles into "topics" — without specifying the topics in advance. Which category?

> **Sample answer:** Unsupervised. No labels. The goal is to discover structure (what topics exist). This is the kind of problem we tackle in **L05**.

**Q7 — Same data, different goal.** Could Sarah's review problem also be tackled with *unsupervised* learning? What would be different?

> **Sample answer:** Yes — she could cluster the 10,000 reviews into, say, 5 groups based on how similar they are. She would not know which group is "positive" or "about sizing" without inspecting a sample. It would be faster to set up (no labels needed) but less useful to Priya, who asked specific labelled questions.

### Part 3 — Workflow

**Q8 — Framing check.** Priya says "90% accuracy is fine." Is the framing complete?

> **Sample answer:** No. Open questions:
> - *Accuracy on what?* Overall, or on the rare "severe complaint" cases specifically?
> - *Who acts on it?* If an angry-customer coupon is auto-sent, a false positive costs real money.
> - *What about topic tagging?* Priya also wanted sizing / quality / delivery — a separate problem.
> Push back is part of the job. Reframing the question is the work.

**Q9 — Train vs test score.** A model scored 98% on the training set and 70% on the held-out test set. What's going on, and what would you do?

> **Sample answer:** Classic **overfitting** — the model memorised the training data instead of learning general patterns. You would try a simpler model, more regularisation, more training data, or fewer / better features. We cover overfitting properly in **L03 and L04**.

**Q10 — Monitoring.** You deploy a fraud detection model. Six months later accuracy has dropped 10%. What happened, and which step of the workflow failed?

> **Sample answer:** Likely **data drift** — fraudsters changed tactics, or normal customer behaviour shifted. Step 7 (monitoring) caught the decay; Step 4 (training) or the data pipeline needs to be rerun on more recent data. This is why "train once and forget" does not work in practice.

---

## Where L01 fits in the course

L01 establishes the vocabulary and the workflow that the rest of M3 lives inside. Every lesson below sits inside the same 7-step workflow you met in `notebooks/04_ml_workflow.ipynb`.

| Lesson | What it adds |
|---|---|
| **L02 — Probability & Statistics for ML** | Answers Sarah's unanswered question: *how sure are we?* Distributions, the Central Limit Theorem, confidence intervals, A/B testing. |
| **L03 — Supervised Learning** | Preprocessing, train/validate splits, metrics, threshold choice. The supervised category, properly. |
| **L04 — Supervised Learning (Advanced)** | Decision trees → random forests → gradient boosting. Hyperparameter tuning. |
| **L05 — Unsupervised Learning** | PCA, k-means, anomaly detection. The category Sarah didn't use this week. |
| **L06 — Time Series** | When the data has a clock — forecasting, decomposition. |
| **L07 — Neural Networks** | Perceptrons → MLPs, gradient descent, PyTorch training loops. |
| **L08 — Computer Vision** | Convolutions, CNNs, transfer learning. |
| **L09 — NLP & Embeddings** | Words → vectors, pretrained embeddings, semantic search. |
| **L10 — Transformers & GenAI** | Attention, LLM APIs, RAG pipelines. |

---

> *"Sarah — your model says most reviews are positive. But are the positive ones actually positive? How do we know we can trust that number?"* — Priya, end of week 2.
>
> Come to L02 ready to help Sarah answer it.
