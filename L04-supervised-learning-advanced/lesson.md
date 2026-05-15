# Lesson 4 — Advanced Supervised Learning: Trees & Ensembles
*Concept reference. Open whenever you want to look up a definition or check the mechanics of an algorithm used in the notebooks.*

> **Where Sarah is.** It's Monday morning of week 4. Marcus's brief from Friday: *"Can you make this model BETTER next week? Try those tree-based models you mentioned."* Same dataset as L03 — `northstar_churn.csv`, 10,000 customers, 11 features, target = churned. By Friday she has to show whether trees and ensembles beat the L03 logistic regression baseline.

---

## The Big Picture — why trees and ensembles dominate tabular ML

Two things make tree-based methods the default toolkit for tabular business data:

1. **They handle non-linear interactions for free.** A logistic regression treats every feature independently and combines them with addition. A tree CAN split on "tenure < 12 months AND returns_per_purchase > 0.10" — capturing the interaction without you hand-engineering it.
2. **They don't need scaled or encoded features.** Trees split on thresholds. `age = 35` is bigger than `age = 25` regardless of whether you standardised it. (You still need to one-hot encode categoricals because trees can't directly compare strings — but no scaling.)

Three algorithms cover ~80% of the tabular ML you'll see in production:

| Algorithm | Type | When to reach for it |
|---|---|---|
| **Decision Tree** | Single tree | Almost never alone. Use as a teaching tool or as a baseline. |
| **Random Forest** | Bagging ensemble (parallel trees) | The default first try. Robust to overfitting. Less tuning needed. |
| **Gradient Boosting** | Boosting ensemble (sequential trees) | The Kaggle winner. Higher accuracy, more sensitive to hyperparameters. |

---

## Part 1 — Decision trees

### How they work

A decision tree learns by repeatedly splitting the data on whichever feature+threshold reduces a measure of *impurity* (Gini or entropy) the most.

```
Is tenure_months < 12?
├── YES → Is returns_per_purchase > 0.10?
│         ├── YES → Predict: CHURN (probability 0.65)
│         └── NO  → Predict: STAY  (probability 0.85)
└── NO  → Is support_tickets_quarter > 3?
          ├── YES → Predict: CHURN (probability 0.40)
          └── NO  → Predict: STAY  (probability 0.95)
```

Each leaf is a probability based on the training labels of customers who land in that leaf.

**Strengths:**
- Captures non-linear interactions automatically
- No scaling, minimal preprocessing
- Easy to visualise and explain

**Weakness:**
- A single deep tree memorises the training data — high variance, overfits badly
- A shallow tree (max_depth=3) is interpretable but underfits

### Hyperparameters that matter

| Hyperparameter | What it controls | Default | Tuning hint |
|---|---|---|---|
| `max_depth` | Maximum number of split levels | None (= grow until pure) | Try 3, 5, 7, 10. Deeper = more variance. |
| `min_samples_leaf` | Minimum samples in a leaf | 1 | Try 5, 10, 20. Higher = less variance. |
| `criterion` | Impurity measure | "gini" | Gini vs entropy gives nearly identical results. Don't tune. |

---

## Part 2 — Random Forest (bagging)

### The idea — averaging many trees

If a single deep tree overfits, train MANY trees on slightly different subsets of the data and AVERAGE their predictions. The variance drops, the bias stays low.

Two sources of "slight difference" between trees:
1. **Bootstrap sampling** — each tree trains on a random sample (with replacement) of the training rows.
2. **Feature subsampling** — at each split, the tree considers only a random subset of features.

The second one is critical. Without it, all the trees would mostly agree (and averaging wouldn't help). With it, trees specialise in different feature combinations and ensemble truly helps.

### Mechanics in sklearn

```python
from sklearn.ensemble import RandomForestClassifier
rf = RandomForestClassifier(
    n_estimators=200,   # number of trees
    max_depth=None,     # let them grow deep — averaging tames the variance
    min_samples_leaf=5, # mild regularisation
    n_jobs=-1,
    random_state=42,
)
```

### Why Random Forest is the default first try

- **Robust to overfitting** — averaging shrinks variance even with many deep trees
- **Few hyperparameters to tune** — `n_estimators`, `max_depth`, `min_samples_leaf` cover most of it
- **Built-in feature importance** — see which features the trees split on most
- **Class-imbalance friendly** — supports `class_weight='balanced'` if your dataset is heavily imbalanced

### Trade-offs vs gradient boosting

| Random Forest | Gradient Boosting |
|---|---|
| Parallel trees, easy to scale | Sequential trees, slower to train |
| Less sensitive to hyperparameters | More sensitive — needs tuning |
| Saturates earlier — adding trees yields diminishing returns | Continues improving with more rounds (and more risk of overfit) |
| Usually 80–90% of the way to the best | Usually the winner with proper tuning |

---

## Part 3 — Gradient Boosting (boosting)

### The idea — sequential error correction

Instead of training trees independently and averaging, train them ONE AT A TIME, each new tree fitting the ERRORS of the previous ensemble.

```
1. Initial prediction = mean of y_train.
2. Compute residuals = y - prediction.
3. Train a small tree to predict the residuals.
4. Update prediction = previous_prediction + learning_rate * tree_prediction.
5. Go to step 2.  (Repeat N rounds.)
```

The result is an ensemble that's progressively more accurate — but ALSO progressively more likely to overfit if you run too many rounds.

### Mechanics in sklearn

```python
from sklearn.ensemble import HistGradientBoostingClassifier
gb = HistGradientBoostingClassifier(
    max_iter=200,        # number of boosting rounds (each = one tree)
    learning_rate=0.1,   # how much each tree contributes
    max_depth=None,      # depth of each tree (often 3-8)
    random_state=42,
)
```

`HistGradientBoostingClassifier` is sklearn's fast histogram-based implementation — same family as XGBoost and LightGBM, and competitive with them for many problems.

### Hyperparameters that matter

| Hyperparameter | What it controls | Tuning hint |
|---|---|---|
| `learning_rate` | How aggressively each new tree corrects errors | Smaller (0.01–0.1) = more trees needed but better generalisation |
| `max_iter` | Total number of trees | Pair with learning_rate. Use `early_stopping=True` to auto-pick. |
| `max_depth` | Depth of each tree | Trees are usually shallow (3–8). Deeper = more variance per round. |
| `l2_regularization` | Penalty on leaf values | Adds regularisation. Try 0.0, 0.1, 1.0. |

### Why gradient boosting wins Kaggle

- Sequential training lets each tree focus on the hardest examples
- Trees can be shallow → fast individually → many rounds → strong overall
- Modern implementations (XGBoost, LightGBM, HistGradientBoosting) are extremely fast

### The danger — overfitting if you don't tune

A gradient boosting model with `max_iter=1000` and `learning_rate=0.1` on a small dataset is a fast path to overfitting. Cross-validate. Use early stopping. Don't trust training accuracy.

---

## Part 4 — Hyperparameter tuning

### Why tune

Default hyperparameters are *reasonable* but rarely *optimal* for your dataset. Tuning is the difference between a 0.85 and a 0.91 F1 — often more impactful than switching algorithms.

### GridSearchCV — exhaustive search

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "model__n_estimators":     [100, 200, 400],
    "model__max_depth":        [None, 5, 10],
    "model__min_samples_leaf": [1, 5, 20],
}
grid = GridSearchCV(
    estimator=pipeline,
    param_grid=param_grid,
    cv=5,
    scoring="f1",
    n_jobs=-1,
)
grid.fit(X_train, y_train)
print(grid.best_params_, grid.best_score_)
```

- **Tries every combination** in the grid (here: 3 × 3 × 3 = 27 settings).
- **Cross-validates each one** (5 folds → 27 × 5 = 135 model fits).
- **Picks the best by mean CV score** on the chosen metric.

`n_jobs=-1` parallelises across CPU cores. Critical for keeping wall-clock time sensible.

### RandomizedSearchCV — sampled search

When the grid is too big to enumerate, sample N random combinations instead:

```python
from sklearn.model_selection import RandomizedSearchCV
search = RandomizedSearchCV(
    estimator=pipeline,
    param_distributions=param_dist,
    n_iter=50,
    cv=5,
    scoring="f1",
    n_jobs=-1,
)
```

In practice, RandomizedSearch finds combinations close to the grid optimum with much less compute.

### What to tune for

The same metric you'll use for the final decision. If business priority is recall, tune for recall. If it's F1, tune for F1. The wrong metric here can lead you to the wrong "best" model.

---

## Part 5 — Friday's recommendation: model comparison

By Thursday evening Sarah has three candidates trained on the same `X_train` / `y_train`:

| Model | Test F1 | Comment |
|---|---|---|
| **Logistic Regression (L03 baseline)** | ~0.325 | The bar to beat |
| **Random Forest (tuned)** | ~0.321 | Easy to train, robust — but barely matches LR here |
| **Gradient Boosting (tuned)** | ~0.335 | Highest F1; needs careful tuning |

**Honest observation:** these three models are within 0.014 F1 of each other on this dataset. That's not a coincidence — when a class-imbalanced problem has limited signal, no algorithm pulls dramatically ahead. The gap widens on richer, less-imbalanced datasets.

The Friday recommendation isn't *automatically* the highest-F1 model. Sarah considers:

| Question | What it implies for the choice |
|---|---|
| **How explainable does the model need to be?** | Pushes toward Random Forest (feature importance is intuitive) over Gradient Boosting (less interpretable) |
| **How much does the engineering team want to support?** | Sklearn-only is simpler than adding XGBoost as a dependency |
| **How sensitive is the team to noisy retraining?** | Random Forest is more stable across retrains; GB can be brittle |
| **Are we beating the L03 baseline by enough to justify the change?** | A 0.335 vs 0.325 may not be worth the operational complexity |

> **Sarah's actual pitch:** *"Gradient Boosting wins by a hair — 0.335 vs LR's 0.325 — and HistGradientBoostingClassifier is already in sklearn so there's no new dependency. I recommend we ship the tuned Gradient Boosting model. Random Forest stays as our fallback if GB starts retraining noisily in production."*

---

## What Marcus asks next

> *"Nice. Now — what about all those customers who DON'T churn but also don't log in for a year? Can we find natural clusters of customer behaviour? Without labels?"*

That question — **can we find structure when there's no target column?** — is the engine of **L05 (Unsupervised Learning).**

---

## Glossary

See [`reference.md`](./reference.md) for a 20-term glossary covering trees, ensembles, and tuning language used in this lesson.
