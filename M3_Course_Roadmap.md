# DSAI M3: Machine Learning & GenAI — Course Roadmap

**Prepared for:** FF, SkillsUnion
**Revised:** April 2026
**Supersedes:** earlier module-numbered roadmap and `M3_Lesson_Sequence_Proposal.md`

---

## Course at a Glance

Ten lessons. Each lesson is **3 hours** of live class time, flipped-classroom format (learners pre-watch / pre-read before class, class time is reinforcement + hands-on practice, post-class is independent application).

Total live time: **30 hours**, plus two group activities (Kaggle Competition after L04, Hackathon after L10).

This is a **general-purpose course**, not industry-specific. Topic selection favours algorithms and techniques that are widely used across industries — finance, retail, healthcare, marketing, operations — over niche or specialist methods.

---

## The Guiding Rule: Core vs Optional

Three hours of class time realistically fits **2–3 core topics** with meaningful hands-on practice. Anything beyond that gets skimmed. So every lesson declares its topics up front:

**🟢 Core** — the 2–3 topics the lesson *will* teach in class. A topic qualifies as Core if it meets at least two of:
- Used in most production ML projects across industries
- Expected by an interviewer or hiring manager at entry level
- A foundation that the next lesson depends on

**🟡 Optional** — valuable topics that don't meet the bar above, or that are covered well enough by library abstractions in practice. These are moved to a per-lesson `optional_extensions.ipynb` notebook in each lesson folder. Self-study only. Not taught in class, not assessed.

This pattern is used consistently across all 10 lessons.

---

## Lesson Map

```
PHASE 1 — FOUNDATIONS
  L01  Intro to Machine Learning
  L02  Probability & Statistics for ML

PHASE 2 — CLASSICAL ML (the workhorse toolkit)
  L03  Supervised Learning Foundations
  L04  Advanced Supervised Learning (Trees & Ensembles)
           ↓
      [ KAGGLE COMPETITION — team activity ]

PHASE 3 — BEYOND LABELS
  L05  Unsupervised Learning
  L06  Time Series Forecasting

PHASE 4 — DEEP LEARNING
  L07  Neural Networks & Deep Learning
  L08  Computer Vision

PHASE 5 — LANGUAGE & GENERATIVE AI
  L09  NLP Fundamentals
  L10  Transformers + Practical GenAI (merged)
           ↓
      [ HACKATHON CAPSTONE — team activity ]
```

---

## Per-Lesson Topic Breakdown

### L01 — Intro to Machine Learning

**Why it matters:** the mental model for the whole course. Without this, the rest of the course is a collection of techniques with no frame.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Supervised / unsupervised / reinforcement mental model | The classification every real project starts with |
| 🟢 Core | Regression vs classification + their standard metrics | The first decision on any ML project |
| 🟢 Core | Run one pre-trained model end-to-end (sentiment demo) | The "aha" — learners *see* what ML does before they learn how |
| 🟡 Optional | Polynomial feature engineering, KNN deep-dive, MLP from scratch, formal train/test split theory | → `optional_extensions.ipynb` |

---

### L02 — Probability & Statistics for ML

**Why it matters:** every ML decision rests on "how sure are we?" Statistics is the vocabulary for answering that question.

> **Scope note:** L01 already introduced descriptive stats intuitively as Sarah explored her review data (shape of a distribution, mean, spread, a correlation). L02 is the formal layer — with the maths, the CLT, and the inferential tools.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Distributions formalised (normal, skew, Z-scores) + why the shape matters for modelling | Builds on L01's intuition; every EDA and every model assumption rests on this |
| 🟢 Core | Confidence intervals + Central Limit Theorem | The "how sure" answer learners will give 100 times in their career |
| 🟢 Core | A/B testing with p-values | The most widely used statistical tool in industry (product, marketing, UX) |
| 🟡 Optional | Bayes' theorem math, t-test formula derivation, bootstrapping theory, full CLT proof | → `optional_extensions.ipynb` |

---

### L03 — Supervised Learning Foundations

**Why it matters:** this lesson has the highest ROI in the course. The preprocessing and evaluation skills here are what entry-level ML analysts actually do all day.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Preprocessing pipeline (scaling, encoding, imputation) | The time sink of every real ML project |
| 🟢 Core | Train/test split + k-fold cross-validation | The correct-answer baseline for trustworthy results |
| 🟢 Core | Confusion matrix + precision/recall/F1 + threshold choice | The metric every production classifier is judged on |
| 🟡 Optional | Bias-variance math, ROC-AUC derivation, learning curves, manual feature engineering exercises | → `optional_extensions.ipynb` |

---

### L04 — Advanced Supervised Learning (Trees & Ensembles)

**Why it matters:** these algorithms win Kaggle, and they also win production. Random Forest and Gradient Boosting are the default toolkit for tabular business data.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | sklearn `Pipeline` (leak-proof end-to-end workflow) | Standard practice; prevents the most common hidden bug |
| 🟢 Core | Random Forest | Still one of the most deployed algorithms in industry |
| 🟢 Core | Gradient Boosting (XGBoost or LightGBM) | The highest-accuracy algorithm for tabular data |
| 🟡 Optional | Ridge/Lasso/ElasticNet (Ridge touched briefly in core), Gini vs Entropy math, bagging-vs-boosting deep theory, GridSearch vs RandomSearch comparison | → `optional_extensions.ipynb` |

**Kaggle Competition** follows this lesson.

---

### L05 — Unsupervised Learning

**Why it matters:** customer segmentation, anomaly detection, and dimensionality reduction are used across marketing, fraud, and operations. Three algorithms cover 90% of real work.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | K-Means clustering | The most deployed clustering algorithm; customer segmentation, document grouping |
| 🟢 Core | PCA for dimensionality reduction | Standard before feeding high-dim data into other models; also for EDA viz |
| 🟢 Core | Isolation Forest for anomaly / fraud detection | Widely used in fraud and time-series sensor data |
| 🟡 Optional | Hierarchical clustering + dendrograms, DBSCAN parameter tuning, t-SNE / UMAP, Z-score / IQR outlier math (also in L02) | → `optional_extensions.ipynb` |

---

### L06 — Time Series Forecasting

**Why it matters:** essential in supply chain, finance, energy, and operations. Even for learners outside those domains, the "trend + seasonality + noise" mental model transfers to many problems.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Trend + seasonality decomposition (STL) | The intuitive mental model that unlocks everything else |
| 🟢 Core | Exponential smoothing / ETS | The pragmatic baseline everyone should know |
| 🟢 Core | Prophet *or* LightGBM with lag features | The modern practical approach used in industry today |
| 🟡 Optional | ARIMA manual p/d/q tuning (use AutoARIMA in the core), SARIMA, stationarity tests, ACF/PACF deep dive | → `optional_extensions.ipynb` |

---

### L07 — Neural Networks & Deep Learning

**Why it matters:** this is the bridge from classical ML to the three domain-specific deep-learning lessons that follow. The goal is intuition, not production-grade neural net engineering.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Perceptron → multi-layer + activations + loss (intuition) | Conceptual foundation for everything in L08–L10 |
| 🟢 Core | Gradient descent + backprop (mountain analogy) | How learning *actually happens* — the mental model that makes the rest make sense |
| 🟢 Core | PyTorch training loop (fit / evaluate / predict pattern) | The hands-on standard; transfers to every downstream DL task |
| 🟡 Optional | NumPy-from-scratch neural net, full chain-rule derivation, weight-init strategies, Adam / SGD variants | → `optional_extensions.ipynb` |

---

### L08 — Computer Vision

**Why it matters:** transfer learning and data augmentation are universally useful whenever images are involved. Other CV techniques are more specialist.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | CNN intuition (filters → pooling → deeper = more abstract) | The minimum needed to work with any vision model |
| 🟢 Core | Transfer learning with a pre-trained model (ResNet / EfficientNet) | The dominant production pattern — real projects almost never train from scratch |
| 🟢 Core | Data augmentation | The standard fix for small datasets, which is every real-world dataset |
| 🟡 Optional | Classical Sobel filters (5-min historical demo), training a CNN from scratch, object detection, segmentation, YOLO | → `optional_extensions.ipynb` |

---

### L09 — NLP Fundamentals

**Why it matters:** every text-based ML project starts with preprocessing + a baseline. Embeddings are the bridge to transformers in L10.

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Text preprocessing pipeline (tokenize, normalize, stopwords) | Required before anything else in NLP |
| 🟢 Core | TF-IDF + text classification pipeline | The baseline that still wins in many production settings |
| 🟢 Core | Word / sentence embeddings (Word2Vec, or modern sentence embeddings) | The bridge to transformers — and the representation layer everyone uses |
| 🟡 Optional | N-gram language models, NER with spaCy, LDA topic modelling, heavy regex cleaning recipes | → `optional_extensions.ipynb` |

---

### L10 — Transformers + Practical GenAI *(merged)*

**Why it matters:** this is the hottest area in the industry in 2026. Every knowledge worker in a data-adjacent role will use LLMs, and every company is building GenAI features. This lesson teaches both *understanding* transformers (first half) and *using* them via APIs (second half).

| | Topic | Why it's here |
|---|---|---|
| 🟢 Core | Transformers + Hugging Face (use a pre-trained model for classification) | The standard library for any transformer work |
| 🟢 Core | Prompt engineering + LLM APIs (OpenAI or Anthropic SDK) | Every knowledge worker will do this |
| 🟢 Core | RAG basics (embed → retrieve → generate) | The standard enterprise GenAI pattern |
| 🟡 Optional | RNN / LSTM gate equations, attention-math derivation, fine-tuning a transformer, LLM-as-judge evaluation, training transformers from scratch | → `optional_extensions.ipynb` |

**Hackathon Capstone** follows this lesson.

---

## Per-Lesson File Template

Every lesson folder follows this structure:

```
L{NN}-{slug}/
  README.md                              ← navigation hub, 3-phase flow, Core vs Optional callout
  setup.md                               ← one-time environment setup
  pre-class.md                           ← Phase 1: videos + mini-exercises
  lesson.md                              ← Phase 2: concept reference
  notebooks/
    Part_1_*.ipynb                       ← Core topic 1
    Part_2_*.ipynb                       ← Core topic 2
    Part_3_*.ipynb                       ← Core topic 3 (if applicable)
    case_study.ipynb                     ← Phase 2: story-driven alternative
    assignment.ipynb                     ← Phase 3: tiered practice + assignment
    optional_extensions.ipynb            ← 🟡 self-study only; demoted topics live here
  reference.md                           ← glossary + further reading
  environment.yml
  assets/                                ← infographics & images
```

The **`optional_extensions.ipynb`** is new in this revision. It:
- Is clearly marked "self-study only — not reviewed in class"
- Opens with a summary of which topics it covers and why each was demoted
- Uses the same business-scenario pattern as the Core notebooks (so it's actually readable, not a dumping ground)
- Has no assignment attached — it is purely for motivated learners who want depth

---

## Topic-Priority Heatmap

### 🟢 Focus — high hands-on time

These topics deliver the most career value and should get the most practice time across the course:

- Data preprocessing pipeline (cleaning, scaling, encoding)
- Model evaluation metrics (confusion matrix, precision/recall, F1, RMSE)
- sklearn Pipeline
- Random Forest
- Gradient Boosting (XGBoost / LightGBM)
- Cross-validation and hyperparameter tuning
- K-Means clustering
- PCA
- Isolation Forest
- Seasonal decomposition (STL) + ETS + Prophet / LightGBM-lag
- PyTorch training loop
- Transfer learning (vision and NLP)
- Transformers via Hugging Face
- Prompt engineering + LLM APIs
- RAG pattern

### 🟡 Optional — self-study only (per-lesson `optional_extensions.ipynb`)

Valuable for curious learners; not assessed, not taught in class:

- Polynomial features, KNN deep-dive, NN from NumPy
- Bayes' theorem math, t-test derivation, bootstrapping theory
- Bias-variance math, ROC-AUC derivation, learning curves
- Ridge / Lasso / ElasticNet deep dive, Gini vs Entropy math
- Hierarchical clustering, DBSCAN tuning, t-SNE / UMAP
- ARIMA manual p/d/q, SARIMA, stationarity tests
- Chain-rule derivation for backprop, weight-init strategies, Adam variants
- Classical edge detection (Sobel), training a CNN from scratch, YOLO
- N-grams, NER, LDA topic modelling
- RNN / LSTM gate equations, attention math, fine-tuning a transformer

---

## What Changed from the Previous Roadmap

The earlier roadmap sketched 11 modules and used a tiered `TIER 1 / TIER 2 / TIER 3` classification. This revision:

1. **Constrains to 10 lessons** to fit the 3-hour-per-lesson reality.
2. **Merges "Advanced NLP" and the proposed "Practical GenAI" into L10** — learners understand a transformer in the first half of L10 and *use* one via API in the second half.
3. **Replaces tier-classification with explicit per-lesson Core / Optional lists** so instructors and learners know exactly what's being taught.
4. **Introduces a per-lesson `optional_extensions.ipynb`** so demoted content has a clear home rather than being scattered in footnotes across lesson files.
5. **Removes domain-specific weighting** — since this is a general course, topic selection is by cross-industry frequency, not by any one industry's priorities.

Position of Time Series (L06) is kept in its current slot; a short "specialist framing" note at the top of L06's README acknowledges that the mental model is universal even if some algorithms are domain-heavy.

---

## Implementation Status

**As of May 2026, all ten lessons are live on GitHub** at https://github.com/flexfengfeng/dsai-m3-ml-genai.

| Lesson | Status |
|---|---|
| L01 — Intro to ML | ✅ Live |
| L02 — Probability & Statistics for ML | ✅ Live |
| L03 — Supervised Learning Foundations | ✅ Live |
| L04 — Advanced Supervised Learning | ✅ Live |
| L05 — Unsupervised Learning | ✅ Live |
| L06 — Time Series Forecasting | ✅ Live |
| L07 — Neural Networks & Deep Learning | ✅ Live |
| L08 — Computer Vision | ✅ Live |
| L09 — NLP Fundamentals | ✅ Live |
| L10 — Transformers + Practical GenAI | ✅ Live |

Each lesson follows the same shape: pre-class notebook + 3 in-class notebooks + assignment + optional_extensions, plus a slide deck and an instructor `HANDOFF_OPTION_A.md`.

The full repo is set up to run on macOS, Windows WSL2, or Google Colab — see [SETUP.md](./SETUP.md) for environment instructions and [HACKATHON_GUIDE.md](./HACKATHON_GUIDE.md) for the end-of-course capstone.

---

## Open Decisions Captured

- **Optional topics location:** per-lesson `optional_extensions.ipynb` in each lesson's `notebooks/` folder. (Decided April 2026.)
- **Industry focus:** general-purpose, not specialist. (Decided April 2026.)
- **L10 structure:** single merged lesson covering Transformers + Practical GenAI, with RNN/LSTM math demoted to optional. (Decided April 2026.)
- **L01 / L02 scope boundary:** L01 introduces descriptive stats concepts *intuitively* as Sarah explores her review data (shape, mean, spread, correlation — no formulas). L02 is the *formal* treatment: distributions with their math, Central Limit Theorem, confidence intervals, hypothesis testing, A/B testing. This prevents the "first lesson is abstract" problem without doubling up content. (Decided April 2026.)
- **Protagonist continuity:** Sarah Chen anchors L01–L05 and L09–L10 where her Customer Experience role fits the content. L06 (time series), L07 (neural networks), and L08 (computer vision) use a different character in the same NorthStar Retail universe — realistic because different problems land on different desks, and avoids forcing Sarah into implausible scenarios. The cast and timeline live in each lesson's `STORY_BIBLE.md`. (Decided April 2026.)
- **L01 anchor task:** sentiment classification of customer reviews. Chosen because a human cannot read 10,000 reviews by hand — this gives the "ML unlocks something impossible" hook that motivates the whole course. Customer churn is a better fit for L03 / L04 as a supervised tabular problem. (Decided April 2026.)
