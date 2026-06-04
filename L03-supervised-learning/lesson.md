# Lesson — L03 Supervised Learning Foundations

> **Chapter 3 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · January 2023.*
> Marcus's question after Sarah's Friday presentation — *"Can you train your own model on NorthStar data?"* — is the brief for the whole week. Sarah has `northstar_churn.csv`: 10,000 customers, 11 features, one target column (`churned`), and Friday to ship a working classifier.

This document is a **short reference** — the lesson itself is taught in the notebooks. Read it for orientation before class, then come back for the takeaways, the threshold-choice checklist, the review questions, and the course map.

---

## How L03 is taught

| Stage | Where to go |
|---|---|
| **Pre-class** | `pre-class.md` + `notebooks/01_monday_morning.ipynb` |
| **In-class — Part 1: Preprocessing** | `notebooks/02_preprocessing.ipynb` |
| **In-class — Part 2: Train / Validate** | `notebooks/03_train_validate.ipynb` |
| **In-class — Part 3: Metrics & Threshold** | `notebooks/04_metrics_threshold.ipynb` |
| **Self-study** | `notebooks/assignment.ipynb` + `notebooks/optional_extensions.ipynb` |
| **Reference & review** | This document |

The notebooks are the spine. Run them in order. Come back here for the consolidated takeaways and the review questions.

---

## Overview

Marcus's L02-closing question — *can you build us a model from NorthStar's own data?* — sets up the first model Sarah truly owns. By Friday she will hold three new tools: a **preprocessing pipeline** (so messy real-world CSVs become numbers a model can learn from, without leakage), **train/validate discipline** (cross-validation, not single-split luck), and **business-aware threshold choice** (the decision that converts a probability into who-gets-called). Every later supervised model in this course is built on the same scaffolding.

---

## Key takeaways

1. **A supervised-ML week is 60–70% preprocessing.** Missing values, scales, categories, leakage. Most of the bugs that "look like the model is bad" are actually preprocessing bugs.
2. **Always wrap preprocessing in a sklearn `Pipeline` + `ColumnTransformer`.** Same transformation at train and inference, zero leakage from test into train, zero copy-paste drift between fit and predict code.
3. **Z-score scale numerical features; one-hot encode categorical ones.** Skip scaling for tree models, but be explicit about it. Use a "was-missing" flag when missingness itself is informative.
4. **Split with `stratify=y` on imbalanced data, then k-fold cross-validate on the training portion only.** The held-out test set stays untouched until the final model is chosen.
5. **Accuracy lies on imbalanced data.** 88% accuracy on a 12% positive class can mean the model predicts "negative" for everyone. Report precision, recall, and F1.
6. **A classifier's threshold is a business decision, not a math one.** 0.5 is rarely right. Pick the threshold that matches the team's capacity and the cost of each error type.
7. **Logistic regression coefficients are interpretable — but only after scaling.** Sign tells direction; magnitude tells strength once features are on comparable units.

---

## Choosing a classifier's threshold — a checklist

Before you set or change a classification threshold, run it through this three-step check:

1. **What is the operational capacity?** How many flagged cases can the team actually act on per week? The threshold's job is to keep flag volume inside that capacity, not to maximise a metric.
2. **What is the cost of each error type?** A false negative (missed churner) and a false positive (wasted intervention) are rarely equally expensive. The cheaper error gets more headroom; the costly one drives the threshold.
3. **Did you decide the threshold from business inputs, or from the test set?** If you tuned the threshold to maximise F1 on the held-out data, you've used that data to make a decision — and you no longer have a clean evaluation. Pick the threshold from capacity + cost, *then* report the metrics at that threshold.

Skip any of these and you'll either drown the team in low-value flags or quietly burn your held-out evaluation.

---

## Check your understanding

Work through these after finishing the three Part notebooks. Attempt each question on your own first.

### Part 1 — Preprocessing

**Q1.** You have a column `subscription_tier` with values `free`, `basic`, `premium`. Should you use label encoding or one-hot encoding?

> *Sample answer:* It's *ordinal* (premium is more than basic which is more than free), so label encoding is actually defensible here. But one-hot encoding will not hurt — and is safer because it doesn't force the model to assume the gap between free and basic equals the gap between basic and premium. For a 3-tier ordinal, either is fine.

**Q2.** Your dataset has `avg_review_polarity` missing for 30% of customers (those who didn't write reviews). What's the best imputation strategy?

> *Sample answer:* The missingness is *informative* — customers who don't write reviews behave differently from those who do. The best approach is to add a `was_missing_polarity` flag AND impute the missing values (say with median 0). That preserves both signals.

**Q3.** You scale your features using `StandardScaler` fit on the full dataset and then split into train/test. What's wrong?

> *Sample answer:* You've leaked test-set information into the training process. The scaler's mean and standard deviation include the test rows. At deployment time you wouldn't have access to the future test data. Always: split first, fit the scaler on the training data only, transform both.

### Part 2 — Training and Validating

**Q4.** Why use `stratify=y` when splitting an imbalanced dataset?

> *Sample answer:* Without stratification, a random split could give you a test set with very few (or very many) positive cases by chance. Stratification guarantees the class proportions in train and test mirror the full dataset, so the test-set evaluation is meaningful.

**Q5.** You did a single 80/20 split and got 87% accuracy. You re-split with a different `random_state` and got 91% accuracy. Which number do you report?

> *Sample answer:* Neither. The 4-point swing between random seeds tells you a single split is too noisy. Use 5-fold cross-validation instead and report the mean ± std.

**Q6.** When you compute `cross_val_score(pipeline, X_train, y_train)`, does sklearn re-fit the preprocessing steps on each fold separately?

> *Sample answer:* Yes — and that's why you must wrap preprocessing INSIDE the pipeline. Each fold becomes a new mini train/test split; the scaler, imputer, and encoder are fit on the fold's train portion only. Without the pipeline, you'd leak.

### Part 3 — Metrics and Threshold

**Q7.** A churn model has TP=120, FP=80, FN=30, TN=770. Compute precision, recall, F1, and accuracy.

> *Sample answer:*
> - Precision = 120 / (120+80) = 0.60
> - Recall = 120 / (120+30) = 0.80
> - F1 = 2 · 0.60 · 0.80 / (0.60+0.80) = 0.686
> - Accuracy = (120+770) / 1000 = 0.89

**Q8.** Your churn model has accuracy = 0.91 but recall = 0.42. What's likely going on?

> *Sample answer:* The dataset is imbalanced. The model is mostly predicting "no churn" — which is right 91% of the time on the data — but missing more than half of the real churners. Accuracy is hiding a major problem. Report precision/recall/F1 instead, and consider lowering the threshold or using a different scoring metric during training.

**Q9.** Marcus asks: "Why don't we just maximise recall? Catch every churner!"

> *Sample answer:* At very low thresholds (say 0.05), you'd flag almost everyone, achieving near-100% recall. But precision would crash — most flags would be false. Operationally, the retention team would waste all their capacity on customers who weren't going to churn anyway, AND miss the most valuable signal (high-confidence flags). The threshold isn't about maximising one metric — it's about matching capacity and cost.

---

## Where L03 fits in the course

L03's scaffolding — pipelines, train/validate splits, precision/recall/F1, threshold choice — is reused by every later model in the course.

| Lesson | How L03 shows up |
|---|---|
| **L04 — Supervised Learning (Advanced)** | Same pipeline, same metrics — but the model becomes a random forest or gradient-boosted tree. Hyperparameter tuning happens inside the same train/validate scaffold. |
| **L05 — Unsupervised Learning** | No labels, no precision/recall — but the "scale numerical / encode categorical / no leakage" preprocessing discipline is identical. |
| **L06 — Time Series** | Train/validate becomes train/forecast: validation must respect time order. The split logic differs; the discipline doesn't. |
| **L07 — Neural Networks** | The training loop replaces `model.fit()`, but the pipeline / metric / threshold stack is unchanged. |
| **L08 — Computer Vision** | Image classifiers report the same precision/recall/F1 as a churn model. Threshold tuning becomes "operating-point selection" for safety-critical applications. |
| **L09 — NLP & Embeddings** | Retrieval evaluation (precision@k, recall@k) is a direct generalisation of L03's precision/recall to top-k results. |
| **L10 — Transformers & GenAI** | LLM outputs scored against held-out evals follow the same train/test discipline. The "threshold" becomes "which prompt or model variant ships." |

---

> *"This is the first model we own. Can you make it better next week?"* — Marcus, after Sarah's Friday presentation.
>
> That question — *how do I improve this model?* — is the engine of **L04 (Supervised Learning, Advanced)**.
