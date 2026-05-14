# Lesson 3 — Supervised Learning Foundations
*Concept reference. Open whenever you want to look up a definition or check the formula behind a concept used in the notebooks.*

> **Where Sarah is.** It's the week after L02. Marcus's question — "*Can you train your own model on NorthStar data?*" — is the brief for this whole week. Sarah has a new file: `northstar_churn.csv`, 10,000 customers, 11 features, and one target column (`churned`: 0 = stayed, 1 = left within 30 days). By Friday she has to show a working classifier, an honest set of metrics, and a recommendation on how to use it.

---

## The Big Picture — three things a supervised-ML week is actually about

Most beginners think the hard part of a supervised-ML project is "the model". It isn't. Across any real project, the time and risk are concentrated in three places:

1. **Preprocessing.** The data arrives as a CSV that no model can ingest. Missing values, mixed types, wildly different scales. Roughly 60–70% of any ML project lives here.
2. **Honest evaluation.** It is *easy* to fool yourself into thinking your model is good by measuring it on data it already saw. The correct evaluation is on held-out data, and there's discipline to doing it right.
3. **Business-aware threshold choice.** A classifier outputs a probability, not a label. Where you put the decision threshold (0.3? 0.5? 0.7?) determines who you flag and what you miss. This is *not* a technical decision — it's a business one.

Each of the three notebooks in this lesson focuses on one of these.

---

## Part 1 — Preprocessing the data

### Why preprocessing matters

Machine-learning models work on numbers. Real datasets contain:

- **Missing values** (a customer who never logged in has no `last_login_days_ago`)
- **Different scales** (`age` is 18–80; `avg_monthly_spend_gbp` ranges 0–500; `returns_per_purchase` is 0.00–0.50)
- **Categorical text** (region: "London", "North", … — not numbers the model can compute with)
- **Outliers** (a few customers with `avg_monthly_spend_gbp = 500` who would dominate any distance-based algorithm)

Feed raw data to a model and at best it crashes; at worst it produces misleading results that look fine.

### Numerical scaling

When features live on different scales, distance- and gradient-based algorithms get fooled into thinking the large-scale features are more important.

> **Example:** A logistic regression weighing `age` (range 18–80) against `avg_monthly_spend_gbp` (range 0–500) will need very different coefficient magnitudes for the same predictive contribution. The optimizer gets confused, training slows down, and small features get drowned out.

**Standard scaling (z-score)**:
```
scaled = (value − mean) / std
```
Each feature becomes mean 0, std 1. Default choice for logistic regression, neural networks, KNN.

**Min-Max scaling**:
```
scaled = (value − min) / (max − min)
```
Each feature becomes 0 to 1. Useful when the algorithm needs bounded input.

**Decision trees don't care about scale.** They split on thresholds, not distances. You can skip scaling for tree-based models — but you still need to scale if any other step in your pipeline assumes scaled input.

### Handling missing values

Three honest options:

1. **Drop rows with missing values** — simple but loses data. Acceptable only if missing values are rare (<5%) AND missing-at-random (not systematically associated with the target).
2. **Impute with a summary statistic** — replace with mean/median for numerical, mode for categorical. Preserves dataset size but reduces variance and can bias the model.
3. **Add a "was-missing" flag column** — keep the imputed value AND add a binary column telling the model "this row was originally missing". Best when missingness itself is informative (e.g., customers who never logged in have NaN for `last_login_days_ago` — and that itself predicts churn).

In sklearn: `SimpleImputer(strategy='mean')` for numerical; `SimpleImputer(strategy='most_frequent')` for categorical. Add `add_indicator=True` to get the was-missing flag for free.

### Categorical encoding

Models compute on numbers. Categorical text must become numeric.

**Label encoding** maps each category to an integer:
```
London → 0, North → 1, South → 2, …
```
Danger: the model assumes Scotland (3) > London (0), implying a fake ordering. Use only for:
- Truly ordinal variables (subscription_tier: `free` → 0, `basic` → 1, `premium` → 2)
- Tree-based models, which don't assume numeric ordering

**One-hot encoding** maps each category to its own 0/1 column:
```
        London  North  South  Scotland  Wales  Ireland
row1  [    1      0      0       0        0      0    ]
row2  [    0      0      1       0        0      0    ]
```
Each row has exactly one 1. No fake ordering. The default choice for most algorithms.

**Rule of thumb:** use one-hot encoding unless you have a specific reason not to (truly ordinal variable, tree-based model with many categories, severe memory constraint).

### sklearn `Pipeline` + `ColumnTransformer`

Doing each preprocessing step by hand on each column is tedious and error-prone. The library bundles this:

```python
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "tenure_months", "avg_monthly_spend_gbp", ...]
categorical_features = ["region", "subscription_tier"]

preprocessor = ColumnTransformer([
    ("num", Pipeline([
        ("impute", SimpleImputer(strategy="median")),
        ("scale",  StandardScaler()),
    ]), numeric_features),
    ("cat", Pipeline([
        ("impute", SimpleImputer(strategy="most_frequent")),
        ("encode", OneHotEncoder(handle_unknown="ignore")),
    ]), categorical_features),
])

full_pipeline = Pipeline([
    ("prep",  preprocessor),
    ("model", LogisticRegression(max_iter=1000)),
])

full_pipeline.fit(X_train, y_train)
preds = full_pipeline.predict(X_test)
```

Why does this matter? Because the **exact same transformation** is applied at training time and at prediction time. Without a pipeline, beginners almost always introduce leakage (using statistics from the test set in preprocessing) or apply different transformations at the two stages, both of which silently destroy your evaluation.

### Quick Check — preprocessing

**Q1.** You have a column `subscription_tier` with values `free`, `basic`, `premium`. Should you use label encoding or one-hot encoding?

*Sample answer:* It's *ordinal* (premium is more than basic which is more than free), so label encoding is actually defensible here. But one-hot encoding will not hurt — and is safer because it doesn't force the model to assume the gap between free and basic equals the gap between basic and premium. For a 3-tier ordinal, either is fine.

**Q2.** Your dataset has `avg_review_polarity` missing for 30% of customers (those who didn't write reviews). What's the best imputation strategy?

*Sample answer:* The missingness is *informative* — customers who don't write reviews behave differently from those who do. The best approach is to add a `was_missing_polarity` flag AND impute the missing values (say with median 0). That preserves both signals.

**Q3.** You scale your features using `StandardScaler` fit on the full dataset and then split into train/test. What's wrong?

*Sample answer:* You've leaked test-set information into the training process. The scaler's mean and standard deviation include the test rows. At deployment time you wouldn't have access to the future test data. Always: split first, fit the scaler on the training data only, transform both.

---

## Part 2 — Training and validating

### The train/test split

Hold out 20–30% of the data before any training. The model never sees this data during fitting; the score on it is your honest estimate of how it'll do on new data.

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
```

`stratify=y` preserves the class proportion in both splits (important when the classes are imbalanced — our churn dataset is 88% non-churners, 12% churners).

### Logistic regression — what it actually is

Despite the name, logistic regression is a **classification** algorithm. Given features X, it outputs a probability between 0 and 1 that the example belongs to the positive class.

The internal formula:
```
z      = β₀ + β₁·x₁ + β₂·x₂ + … + βₙ·xₙ        (a weighted sum, like linear regression)
prob   = 1 / (1 + e⁻ᶻ)                          (squash z into [0,1] using the sigmoid)
label  = 1 if prob ≥ threshold else 0           (default threshold: 0.5)
```

Two key implications:

1. **The output is a probability, not just a label.** That probability is more informative than the binary label — and it's what makes business-aware threshold choice (Part 3) possible.
2. **Coefficients are interpretable.** A positive `β` for a feature means: higher feature → higher predicted probability of the positive class. Magnitude tells you how strong the effect is — but only when features are scaled to comparable units.

> **Sarah's interpretation, in plain English:** "*The coefficient on `returns_per_purchase` is +0.92 — higher return rates strongly increase the predicted probability of churn. The coefficient on `subscription_tier_premium` is −0.45 — premium subscribers are less likely to churn, after controlling for everything else.*"

### k-fold cross-validation

A single train/test split gives one number. If you got unlucky and the test set happened to be hard (or easy), the number lies.

k-fold cross-validation splits the training data into k equal folds (typically k=5). Train k times — each time use k–1 folds for training, the held-out fold for validation. Average the k validation scores. That mean is a much more stable estimate of model quality.

```python
from sklearn.model_selection import cross_val_score
scores = cross_val_score(full_pipeline, X_train, y_train, cv=5, scoring="accuracy")
print(f"CV accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
```

**Important:** the cross-validation happens on the *training* data. The test set stays untouched until you've selected your final model.

### Quick Check — training and validating

**Q1.** Why use `stratify=y` when splitting an imbalanced dataset?

*Sample answer:* Without stratification, a random split could give you a test set with very few (or very many) positive cases by chance. Stratification guarantees the class proportions in train and test mirror the full dataset, so the test-set evaluation is meaningful.

**Q2.** You did a single 80/20 split and got 87% accuracy. You re-split with a different `random_state` and got 91% accuracy. Which number do you report?

*Sample answer:* Neither. The 4-point swing between random seeds tells you a single split is too noisy. Use 5-fold cross-validation instead and report the mean ± std.

**Q3.** When you compute `cross_val_score(pipeline, X_train, y_train)`, does sklearn re-fit the preprocessing steps on each fold separately?

*Sample answer:* Yes — and that's why you must wrap preprocessing INSIDE the pipeline. Each fold becomes a new mini train/test split; the scaler, imputer, and encoder are fit on the fold's train portion only. Without the pipeline, you'd leak.

---

## Part 3 — Metrics and threshold choice

### Why accuracy alone fails

Our churn dataset is 88% non-churners. A "model" that always predicts "non-churner" achieves 88% accuracy — and is utterly useless. Accuracy hides what the model is actually doing.

### The confusion matrix

For any binary classifier, group every prediction into one of four bins:

|  | Predicted Positive (churn) | Predicted Negative (stay) |
|---|---|---|
| **Actually Positive** (churned) | TP — true positive | FN — false negative |
| **Actually Negative** (stayed) | FP — false positive | TN — true negative |

- **TP** — we flagged a churner correctly. ✓
- **FN** — we said "safe", but they churned. ✗ The painful one if you care about retention.
- **FP** — we flagged a stayer as a churn risk. ✗ Operational cost — we wasted intervention spend.
- **TN** — we correctly let a stayer alone. ✓

### Precision vs recall

```
Precision = TP / (TP + FP)
            "Of everyone we flagged, what fraction were actually churning?"

Recall    = TP / (TP + FN)
            "Of all actual churners, what fraction did we catch?"
```

**They trade off.** Tighten your threshold (only flag the very-high-probability cases): precision rises, recall falls. Loosen it: recall rises, precision falls. There is no "best" — it depends on the business cost of each error.

| Business priority | What you want | Real-world examples |
|---|---|---|
| **High recall** (catch most of them) | Even at the cost of more false alarms | Cancer screening, fraud detection, security alerts |
| **High precision** (don't bother people falsely) | Even at the cost of missing some | Spam filters, automated hiring, "premium customer" tagging |
| **Balanced** | Use F1 = harmonic mean of the two | When both errors have meaningful cost |

### F1 score

The harmonic mean of precision and recall:
```
F1 = 2 · (Precision · Recall) / (Precision + Recall)
```
Ranges 0 to 1. Use F1 when you want a single number that balances both kinds of error. (For very imbalanced classes, also look at *PR-AUC* — see `optional_extensions.ipynb`.)

### Threshold choice — the operational decision

Logistic regression outputs a probability. To turn that into a label you pick a threshold (default 0.5). But 0.5 is rarely the right business choice.

> **Sarah's example.** NorthStar's retention team has the capacity to call about 1,500 customers a month. Predicting churn at threshold 0.5 might surface 800 customers — they have spare capacity. Lowering the threshold to 0.3 raises the count to 1,400 — closer to capacity — and increases recall (catches more real churners) at the cost of precision (more wasted calls).

A threshold decision is a triangle: **operational capacity ↔ recall ↔ precision**. Don't choose it from the math; choose it from the constraint.

### Quick Check — metrics and threshold

**Q1.** A churn model has TP=120, FP=80, FN=30, TN=770. Compute precision, recall, F1, and accuracy.

*Sample answer:*
- Precision = 120 / (120+80) = 0.60
- Recall = 120 / (120+30) = 0.80
- F1 = 2 · 0.60 · 0.80 / (0.60+0.80) = 0.686
- Accuracy = (120+770) / 1000 = 0.89

**Q2.** Your churn model has accuracy = 0.91 but recall = 0.42. What's likely going on?

*Sample answer:* The dataset is imbalanced. The model is mostly predicting "no churn" — which is right 91% of the time on the data — but missing more than half of the real churners. Accuracy is hiding a major problem. Report precision/recall/F1 instead, and consider lowering the threshold or using a different scoring metric during training.

**Q3.** Marcus asks: "Why don't we just maximise recall? Catch every churner!"

*Sample answer:* At very low thresholds (say 0.05), you'd flag almost everyone, achieving near-100% recall. But precision would crash — most flags would be false. Operationally, the retention team would waste all their capacity on customers who weren't going to churn anyway, AND miss the most valuable signal (high-confidence flags). The threshold isn't about maximising one metric — it's about matching capacity and cost.

---

## Friday — what Sarah presents

Sarah walks into Marcus's office with a one-pager:

| What | Result |
|---|---|
| **Pipeline** | One sklearn `Pipeline` — impute · scale · one-hot encode · logistic regression. Same code path at training and prediction. |
| **Cross-validated accuracy** | 89% ± 1% (5-fold). But accuracy isn't the right metric here — see below. |
| **At threshold 0.5** | Precision 0.62 · Recall 0.42 · F1 0.50. Model is *cautious* — it flags only when very sure. |
| **At threshold 0.3** | Precision 0.45 · Recall 0.74 · F1 0.56. Catches more real churners at the cost of more false alarms. |
| **Recommended threshold** | **0.3.** Retention team has capacity for ~1,500 calls/month. Threshold 0.3 surfaces ~1,400. Catches 74% of real churners. Accepts that ~55% of the calls are to customers who weren't going to leave. |

Marcus nods. *"This is the first model we own. Can you make it better next week?"*

That question — **how do I improve this model?** — is the engine of **L04 (Advanced Supervised Learning).**

---

## Glossary

See [`reference.md`](./reference.md) for a 20-term glossary covering preprocessing, training, and evaluation language used in this lesson.
