# Pre-class — L04 Trees & Ensembles

**Time:** about 75 minutes. Do this before the live session.

**Goal:** Understand bagging vs boosting at the conceptual level so the in-class notebooks land. Arrive ready to compare algorithms.

---

## What you're walking into

Last Friday Sarah presented her L03 logistic regression to Marcus. The result:

- **Accuracy:** 0.88 (but irrelevant — baseline is 0.88 too)
- **At threshold 0.25 (capacity-based):** Precision 0.30 · Recall 0.26 · F1 0.28

Marcus's response: *"Nice. Now — make it BETTER. Try those tree-based models you mentioned."*

This week Sarah trains **Random Forest** and **Gradient Boosting** on the same data and tunes both with cross-validation. By Friday she has to defend a model choice with honest evidence.

---

## Task 1 (~20 min) — Open `01_monday_morning.ipynb`

This notebook re-trains the L03 logistic regression so you can see the bar we're trying to beat. Then it shows the first decision tree on the same data — and how it overfits.

**Open** `notebooks/01_monday_morning.ipynb` and **run every cell**.

As you go, jot down:
- The L03 F1 (the bar to beat)
- How big the train-vs-test accuracy gap is for a deep decision tree
- One feature the tree splits on first (often the strongest predictor)

---

## Task 2 (~25 min) — Watch two videos

### Video 1 — Random Forest, Clearly Explained (StatQuest, 10 min)

[https://www.youtube.com/watch?v=J4Wdy0Wc_xQ](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ)

Why it matters: Random Forest is the default first try for any tabular ML problem. Knowing why "many random trees" beats "one big tree" is the conceptual leap.

**Mini-exercise:** explain in one sentence why we use *feature subsampling* at each split (not just bootstrap sampling of rows).

> **Sample answer:** If all trees see all features, they'll all prefer the same split at the top of the tree, and the ensemble degenerates into N copies of nearly the same model. Subsampling features forces trees to specialise, which is what makes averaging genuinely reduce variance.

### Video 2 — Gradient Boost, Part 1: Regression Main Ideas (StatQuest, 15 min)

[https://www.youtube.com/watch?v=3CC4N4z3GJc](https://www.youtube.com/watch?v=3CC4N4z3GJc)

Why it matters: Gradient Boosting is the algorithm that wins Kaggle and is the highest-accuracy tabular ML method available. The video is on regression but the *idea* — sequentially fit the residuals — is identical for classification.

**Mini-exercise:** why does each new tree in gradient boosting fit the *residuals* (errors) of the previous prediction, not the original target?

> **Sample answer:** Fitting the residuals is mathematically equivalent to taking a gradient step down the loss surface. Each tree corrects what the current ensemble got wrong. If you fit each new tree to the original target, you'd just average like Random Forest — different algorithm entirely.

---

## Task 3 (~30 min) — Quick exercises

### Exercise 1 — Bagging vs Boosting in 3 sentences each

Without re-watching the videos:
- What does *bagging* do?
- What does *boosting* do?
- Which one is more sensitive to hyperparameter choice?

> **Sample answers:**
> - **Bagging:** train many trees in PARALLEL on bootstrap samples of the data, then average their predictions. Reduces variance.
> - **Boosting:** train many trees SEQUENTIALLY, each fitting the errors of the previous ensemble. Reduces bias AND variance — at the cost of more careful tuning.
> - **Boosting is more sensitive.** Random Forest "just works" with defaults; gradient boosting needs `learning_rate`, `max_iter`, and `max_depth` chosen carefully.

### Exercise 2 — Diagnose this output

A learner trains Random Forest and reports: training accuracy 1.00, test accuracy 0.78. They want to lower `n_estimators` to "make the model simpler". Are they right?

> **Sample answer:** No. The training/test gap means the trees are deep enough to memorise the training data. Reducing `n_estimators` (number of trees) makes the model NOISIER, not simpler — fewer trees = less averaging = MORE variance. The right move is to constrain individual trees with `max_depth` or `min_samples_leaf`. With `min_samples_leaf=20`, you'd see test accuracy improve and training accuracy drop slightly.

### Exercise 3 — Pick a tuning grid

You want to tune a `HistGradientBoostingClassifier`. You can run ~50 model fits in your time budget. Which set of hyperparameters should you grid-search?

Options:
- A) `learning_rate` ∈ {0.01, 0.05, 0.1, 0.2}, `max_iter` ∈ {100, 200, 400}, `max_depth` ∈ {None, 4, 8} → 4×3×3 = 36 combos × 5-fold CV = 180 fits. TOO MANY.
- B) `learning_rate` ∈ {0.05, 0.1}, `max_iter` ∈ {200, 400}, `max_depth` ∈ {None, 6} → 2×2×2 = 8 combos × 5-fold = 40 fits. FITS.
- C) `learning_rate` ∈ {0.1}, `max_iter` ∈ {1000} → 1 combo × 5-fold = 5 fits. UNDERUTILISES BUDGET.

> **Sample answer:** B. A grid that uses ~80–100% of your fit budget is the right size. C wastes time you could use to explore. A is too greedy and forces you to wait. A common adjustment: drop `max_iter` from the grid entirely and use `early_stopping=True` so the model picks it automatically — freeing budget for more `learning_rate` × `max_depth` combinations.

---

## Active-engagement tips

**During pre-study:**
- Notice that *every* algorithm here uses the same sklearn `Pipeline` interface. The L03 muscle memory transfers.
- The fundamental question this week is "Random Forest vs Gradient Boosting" — keep that comparison in your head.

**In-class:**
- Be ready to argue for ONE model over the others. The Friday recommendation isn't automatic — it's a judgement call.

---

## Bring to the session

By the time you walk in, you should have:

1. ✅ Run `01_monday_morning.ipynb` end-to-end without errors
2. ✅ Watched both StatQuest videos (or read the equivalent)
3. ✅ Your three exercise answers ready
4. ✅ One specific question — anything that didn't fully click

See you Tuesday morning at Sarah's desk.
