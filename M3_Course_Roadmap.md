# DSAI M3: Machine Learning & GenAI — Course Roadmap

Your map of the 10-lesson journey — what each lesson teaches, why it's there, and what's core versus optional.

---

## Course at a Glance

Ten lessons. Each lesson is **3 hours** of live class time, flipped-classroom format (learners pre-watch / pre-read before class, class time is reinforcement + hands-on practice, post-class is independent application).

Total live time: **30 hours**. The end-of-module project is a **Kaggle competition** (default); a team-based **Hackathon** is offered as an optional alternative.

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
      [ END-OF-MODULE PROJECT — Kaggle competition (default)
        or optional team Hackathon ]
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

The **end-of-module project** follows this lesson — a Kaggle competition by default, or an optional team Hackathon (see [HACKATHON_GUIDE.md](./HACKATHON_GUIDE.md)).

---

## What's in Each Lesson Folder

Every lesson follows the same structure, so once you know one, you know them all:

```
L{NN}-{slug}/
  README.md                  ← start here: orientation + how to work through the lesson
  setup.md                   ← environment setup for this lesson
  pre-class.md               ← do this before class (~25 min): run one notebook + reflect
  lesson.md                  ← the full concept reference & narrative
  reference.md               ← glossary + optional further reading & watching
  notebooks/
    01_monday_morning.ipynb  ← the opening scenario
    02_*.ipynb               ← core topic notebooks (run in class)
    03_*.ipynb
    04_*.ipynb
    assignment.ipynb         ← Foundational + Advanced tracks (pick one)
    optional_extensions.ipynb← self-study only — extra depth, not taught or assessed
  slides/                    ← the lecture deck
  environment.yml            ← conda environment spec
```

The **`optional_extensions.ipynb`** in each lesson is purely for motivated learners who want more depth. It's clearly marked self-study only, isn't reviewed in class, and has no assignment attached.

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

## Running the Notebooks

The full repo is set up to run on **macOS, Windows (WSL2), or Google Colab** — see [SETUP.md](./SETUP.md) for environment instructions. For lessons that need a GPU (L08, L10), Colab is the easiest option.

When you finish the ten lessons, the **end-of-module project** is a Kaggle competition (default) or an optional team Hackathon — see [HACKATHON_GUIDE.md](./HACKATHON_GUIDE.md).
