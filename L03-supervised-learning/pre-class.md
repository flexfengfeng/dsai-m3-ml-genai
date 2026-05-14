# Pre-class — L03 Supervised Learning Foundations

**Time:** about 75 minutes. Do this before the live session.

**Goal:** Open the new NorthStar churn dataset. See the messiness of real data. Form one question you want answered in class.

---

## What you're walking into

Last Friday, Marcus asked Sarah: *"Can you build us a model that predicts churn from our OWN customer data?"*

He has just dropped a new file on her desk: `notebooks/data/northstar_churn.csv`. 10,000 rows, 12 columns — one row per customer, one column is the target (`churned`: did they cancel within 30 days). The other 11 columns are everything NorthStar knows about each customer: age, tenure, region, subscription tier, spending behaviour, return rate, support tickets, last login, and — neat touch — the average sentiment score from the L01 sentiment model on every review that customer wrote.

This week Sarah has to:

1. Clean the data so a model can use it.
2. Train her first model from scratch.
3. Measure it honestly — and decide where to set the threshold for action.

Today (pre-class) is just step zero: **open the file, look around, form your own opinion before class**.

---

## Task 1 (~25 min) — Open `01_monday_morning.ipynb`

This notebook is short. It loads the dataset, shows you the structure, and surfaces the things that will need to be handled before any model can be trained.

**Open** `notebooks/01_monday_morning.ipynb` and **run every cell** from top to bottom.

As you go, jot down:
- One column whose values look "noisy" or "weird"
- One column you'd *trust* most as a churn predictor (gut feel — you'll find out later)
- One column with missing values, and a guess about why those rows are missing

---

## Task 2 (~25 min) — Watch two videos

### Video 1 — Logistic Regression, Clearly Explained (StatQuest, 9 min)

[https://www.youtube.com/watch?v=yIYKR4sgzI8](https://www.youtube.com/watch?v=yIYKR4sgzI8)

Why it matters: Logistic regression is the model Sarah will train. Despite the name, it's a *classification* algorithm — it outputs a probability of the positive class, not a continuous number. This 9-minute video is the clearest 9 minutes of statistics video on the internet.

**Mini-exercise:** in your own words, explain why we use a sigmoid function instead of just letting the linear regression output be the "probability".

> **Sample answer:** A linear function can output anything from −∞ to +∞. Probabilities must be between 0 and 1. The sigmoid squashes any real number into that range — and does it smoothly, with the steepest slope near 0.5 (where the model is most uncertain).

### Video 2 — Bias-Variance Tradeoff (StatQuest, 6 min)

[https://www.youtube.com/watch?v=EuBBz3bI-aA](https://www.youtube.com/watch?v=EuBBz3bI-aA)

Why it matters: when you train a model, you have to decide how complex to make it. Too simple → underfits. Too complex → overfits. The tradeoff between bias and variance is the single most useful mental frame for that decision.

**Mini-exercise:** which is worse: high bias or high variance? Justify.

> **Sample answer:** Neither is universally worse — they're a tradeoff. High bias means even your training data is poorly fit, which is easy to spot and fix (try a richer model). High variance means training looks great but test is bad, which is harder to spot and more dangerous in production. In practice, most beginners err toward high variance because complex models feel "smart" — so the rule of thumb is: start with the simplest model that works, then add complexity only if test performance demands it.

---

## Task 3 (~25 min) — A few quick conceptual exercises

### Exercise 1 — Pick the right encoding

You have three categorical columns in a dataset:
- `subscription_tier` with values `free`, `basic`, `premium`
- `region` with values `London`, `North`, `South`, `Scotland`, `Wales`, `Ireland`
- `last_purchase_category` with 50 unique values

For each, decide: label encoding or one-hot encoding?

> **Sample answers:**
> - `subscription_tier`: technically ordinal (premium > basic > free), so label encoding (0/1/2) is defensible. But one-hot also works. Either is fine for a 3-tier ordinal.
> - `region`: pure nominal — no ordering. One-hot encoding.
> - `last_purchase_category` (50 values): one-hot would explode to 50 columns. For tree-based models, use label encoding. For logistic regression, consider grouping rare categories first ("top 10 + other") and one-hot the result.

### Exercise 2 — Spot the leakage

Read this code and find the bug:

```python
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2)
```

> **Sample answer:** The scaler is fit on the FULL dataset, *including the test rows*. So the test rows' values influence the mean and std used during training. This is data leakage — your test performance will be optimistic. Fix: split first, then `scaler.fit_transform(X_train)` and `scaler.transform(X_test)`. Even better: wrap everything in a `Pipeline`.

### Exercise 3 — Read a confusion matrix

A churn model on 1,000 test customers produced:
- TP = 90  (correctly flagged churners)
- FP = 110 (wrongly flagged stayers)
- FN = 50  (missed real churners)
- TN = 750 (correctly let stayers alone)

Compute accuracy, precision, recall, and F1. Which one is most misleading here?

> **Sample answers:**
> - Accuracy = (90 + 750) / 1000 = 0.84
> - Precision = 90 / (90 + 110) = 0.45
> - Recall = 90 / (90 + 50) = 0.64
> - F1 = 2 · 0.45 · 0.64 / (0.45 + 0.64) = 0.53
>
> Accuracy (0.84) is the most misleading — it makes the model sound great. In reality precision is poor (less than half of the flagged customers actually churned) and recall is mediocre (over a third of the real churners were missed). Pull out F1 or report precision and recall separately.

---

## Active-engagement tips

**During pre-study:**
- Hover over each column name and ask: "what kind of preprocessing will this need?"
- Resist the temptation to start modelling. Today is just data familiarisation.

**In-class:**
- Be ready to defend your gut-feel "trust column" from Task 1 when we look at the trained model's coefficients in notebook 03.
- Have one specific question from these exercises ready to ask. Live time with the instructor is best spent on judgement calls, not syntax.

---

## Bring to the session

By the time you walk in, you should have:

1. ✅ Run `01_monday_morning.ipynb` end-to-end without errors
2. ✅ Watched both 5–10 minute videos
3. ✅ Your three "noisy / trusted / missing" column picks ready for the opening discussion
4. ✅ One specific question — anything from this guide that didn't fully click

---

## What happens in class

**Phase 2 (~3 hrs in class):**

- ~90 min slides — instructor walks through preprocessing, training, metrics, and threshold choice using `slides/L03_slides.pptx`. Speaker notes are inline.
- ~90 min hands-on — three notebooks (`02_preprocessing.ipynb` → `03_train_validate.ipynb` → `04_metrics_threshold.ipynb`), Core sections only. Each ends at a clearly marked 🟡 Extension boundary.

**Phase 3 (after class, self-paced):**

- `assignment.ipynb` — Lakeside Bank scenario, three tiers of practice, then three independent exercises in a hospital domain
- `optional_extensions.ipynb` — bias-variance math, ROC-AUC, learning curves, manual feature engineering

See you Tuesday morning at Sarah's desk.
