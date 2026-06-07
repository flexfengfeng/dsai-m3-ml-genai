# L04 — Advanced Supervised Learning (Trees & Ensembles)

> *Sarah Chen's fourth week at NorthStar Retail. Friday last week Marcus asked: "Can you make this model BETTER next week? Try those tree-based models you mentioned." This week she does.*
> By the end of this lesson you will know how to train Random Forest and Gradient Boosting classifiers, how to tune their hyperparameters with cross-validation, and how to choose between competing models with honest evidence.

---

## Before you start — environment setup

> **If this is your first time with this course, follow the [Setup Guide →](./setup.md)** to install the Python environment. It takes 10–15 minutes.
>
> **Already set up from L01–L03?** Your `dsai-m3` environment has everything (`scikit-learn` is included).

---

## Where L04 fits

| Lesson | What you build | What you carry forward |
|---|---|---|
| **L03 — Supervised Foundations** | First trained model — logistic regression on NorthStar churn | A working pipeline (preprocess + train + evaluate) — and Marcus's question: *"Can you make it better?"* |
| **L04 — Trees & Ensembles** *(you are here)* | Random Forest + Gradient Boosting on the same data — and tune them properly | A toolkit that wins Kaggle and wins production |
| **Kaggle Competition** | Apply this toolkit on a held-out scoring split | First public leaderboard ranking |
| **L05 — Unsupervised Learning** | Find structure without labels | Clustering and dimensionality reduction |

**The narrative thread:** Sarah's L03 logistic regression scored recall = 26% at the chosen threshold. This week she has to beat it.

---

## What you will be able to do by the end

- **Explain** why decision trees beat linear models on data with strong feature interactions — and where they fall over
- **Train** a Random Forest classifier and explain the bagging idea that makes it more stable than a single tree
- **Train** a Gradient Boosting classifier and explain the sequential-error-correction idea that makes it consistently the winning algorithm on tabular data
- **Tune** hyperparameters with `GridSearchCV` and pick a final model with cross-validated evidence
- **Compare** competing models honestly — same data, same metric, same threshold — and recommend one to a non-technical stakeholder

---

## How class time is structured (~3 hrs)

| Phase | Time | Format |
|---|---|---|
| Slide-driven lecture | ~90 min | Instructor uses [`slides/L04_slides.pptx`](./slides/L04_slides.pptx) (~28 slides with speaker notes) |
| Hands-on code-alongs | ~90 min | Three notebooks (~25–30 min each) — Core sections only |
| (Self-study after class) | self-paced | Each notebook has a 🟡 Extension section + the Kaggle assignment |

**Why this structure?** Same Option A pacing as L01–L03: ~1.5 hours of slides + ~1.5 hours of hands-on with clearly marked 🟡 Extension boundaries.

---

## Your learning path

### Phase 1 — Before class: self-study (~25 min)

**Goal:** Understand bagging vs boosting at the conceptual level. Arrive at class ready to compare algorithms.

**Start here →** [**pre-class.md**](./pre-class.md)

You will:
- Run `notebooks/01_monday_morning.ipynb` (~15 min) — Marcus's brief, the L03 baseline you're trying to beat
- Watch two short videos (StatQuest on Random Forest and Gradient Boosting)
- Try three mini-exercises with sample answers

---

### Phase 2 — In class: concept review + hands-on notebooks (~3 hrs)

**Short reference & review →** [**lesson.md**](./lesson.md) (overview, key takeaways, model-shipping checklist, L05→L10 course map)

**Notebooks — run in order:**

| # | Notebook | Sarah's day | What you explore |
|---|---|---|---|
| 02 | [`02_decision_tree_to_forest.ipynb`](./notebooks/02_decision_tree_to_forest.ipynb) | Tuesday | Decision tree mechanics · why a single tree overfits · Random Forest (bagging) |
| 03 | [`03_gradient_boosting.ipynb`](./notebooks/03_gradient_boosting.ipynb) | Wednesday | Boosting concept · HistGradientBoostingClassifier · feature importance |
| 04 | [`04_tuning_and_comparison.ipynb`](./notebooks/04_tuning_and_comparison.ipynb) | Thursday | GridSearchCV · model comparison · the Friday-morning recommendation |

Each notebook opens with a business scenario, guides you through the code with **Pause & Predict** prompts, and ends at a clearly marked 🟡 Extension boundary.

---

### Phase 3 — After class: Kaggle competition + assignment (self-paced)

**Assignment →** [`notebooks/assignment.ipynb`](./notebooks/assignment.ipynb)

A Kaggle-style competition on a held-out NorthStar-churn scoring set. Three tiers:

- **Tier 1** — Beat F1 ≈ 0.20 on the scoring set with any default-settings model
- **Tier 2** — Beat F1 ≈ 0.30 (roughly a tuned Random Forest / default Gradient Boosting)
- **Tier 3** — Open optimisation: beat F1 ≈ 0.35 with your best tuned model

Followed by an independent banking-fraud detection scenario.

**Further reading →** [**reference.md**](./reference.md)

---

## Core vs Optional — what this lesson teaches

**🟢 Core (taught in class):**
- sklearn `Pipeline` end-to-end workflow (recap + extended with new estimators)
- Random Forest classifier
- Gradient Boosting (HistGradientBoostingClassifier)
- Hyperparameter tuning with `GridSearchCV`

**🟡 Optional (self-study, not assessed):**
- Ridge / Lasso / ElasticNet regularisation for linear models
- Gini vs Entropy split criteria — the math behind tree splits
- GridSearch vs RandomizedSearch comparison
- XGBoost / LightGBM (if installed)

Optional material lives in [`notebooks/optional_extensions.ipynb`](./notebooks/optional_extensions.ipynb).

---

## File map

```
README.md                              ← You are here
setup.md                               ← One-time environment setup (same dsai-m3 env)
pre-class.md                           ← Phase 1: 75-min self-study guide
lesson.md                              ← Short reference: overview, takeaways, model-shipping checklist, course map
reference.md                           ← Phase 3: Further reading + glossary
environment.yml                        ← Conda environment spec (no new packages vs L03)
slides/
  L04_slides.pptx                      ← Instructor slide deck (~28 slides, speaker notes inline)
  slides_outline.md                    ← Slide-by-slide outline + section timings
notebooks/
  data/
    northstar_churn.csv                ← Same dataset as L03 (story continuity)
  01_monday_morning.ipynb              ← Pre-class hook: revisit the L03 baseline (~15 min, before class)
  02_decision_tree_to_forest.ipynb     ← Part 1: Tree → Random Forest (Tuesday, in class)
  03_gradient_boosting.ipynb           ← Part 2: Gradient Boosting (Wednesday, in class)
  04_tuning_and_comparison.ipynb       ← Part 3: GridSearch + model comparison (Thursday, in class)
  assignment.ipynb                     ← Kaggle-style competition (after class)
  optional_extensions.ipynb            ← 🟡 Optional: Ridge/Lasso · Gini vs Entropy · GridSearch vs Random
```
