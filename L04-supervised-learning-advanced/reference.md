# L04 — Further Reading & Glossary

## Further reading

### Videos (free)

- **StatQuest — Random Forests**
  [https://www.youtube.com/watch?v=J4Wdy0Wc_xQ](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ)
  The conceptual leap from "one tree" to "a forest of trees" with feature subsampling.

- **StatQuest — Gradient Boost (4-video series)**
  Part 1 (regression intuition): [https://www.youtube.com/watch?v=3CC4N4z3GJc](https://www.youtube.com/watch?v=3CC4N4z3GJc)
  Part 3 (classification): [https://www.youtube.com/watch?v=jxuNLH5dXCs](https://www.youtube.com/watch?v=jxuNLH5dXCs)

- **StatQuest — XGBoost**
  [https://www.youtube.com/watch?v=OtD8wVaFm6E](https://www.youtube.com/watch?v=OtD8wVaFm6E)
  Industry-standard gradient boosting library, builds on the StatQuest GB series.

### Interactive / official docs

- **scikit-learn — Ensemble methods**
  [https://scikit-learn.org/stable/modules/ensemble.html](https://scikit-learn.org/stable/modules/ensemble.html)

- **scikit-learn — Hyperparameter tuning**
  [https://scikit-learn.org/stable/modules/grid_search.html](https://scikit-learn.org/stable/modules/grid_search.html)

- **Kaggle Learn — Intermediate Machine Learning**
  [https://www.kaggle.com/learn/intermediate-machine-learning](https://www.kaggle.com/learn/intermediate-machine-learning)
  Practical 5-lesson course covering pipelines, missing values, categorical encoding, and gradient boosting — all in the same flavour as this lesson.

### Books

- **Hands-On Machine Learning** by Aurélien Géron — chapters 6 and 7.
- **The Elements of Statistical Learning** by Hastie/Tibshirani/Friedman — chapter 10 (boosting). More mathematical.

---

## Glossary (20 key terms)

- **Bagging** — Bootstrap Aggregating. Train many models in parallel on bootstrap samples of the data, then average their predictions. Reduces variance. Random Forest is the canonical bagging algorithm.

- **Bootstrap sample** — A random sample of N rows drawn WITH replacement from the training set, where N equals the original size. About 63% of rows appear at least once; the rest are duplicates.

- **Boosting** — Train many models SEQUENTIALLY, each one focused on the errors of the previous ensemble. Reduces both bias and variance, at the cost of more careful tuning. AdaBoost and Gradient Boosting are the two main families.

- **Class weight** — A multiplier applied to the loss for each class. `class_weight='balanced'` inversely weighs classes by their frequency — useful for imbalanced datasets like churn.

- **Cross-validation (k-fold)** — Split the training data into k equal folds. Train k times, each using k–1 folds for training and the remaining fold for validation. The mean of the k scores is your stable estimate.

- **Decision tree** — A model that learns to split the data on features+thresholds. Each leaf is a prediction. Strong baseline; rarely used alone in production.

- **Early stopping** — Stop adding trees to a boosted model as soon as the validation score stops improving. Prevents overfitting. Enable with `early_stopping=True` in `HistGradientBoostingClassifier`.

- **Feature importance** — A per-feature score that estimates how much that feature contributed to the model's predictions. Two flavours: *impurity-based* (the default; can be misleading) and *permutation-based* (slower; more reliable).

- **Gradient Boosting** — Boosting where each new tree fits the residuals (errors) of the previous ensemble's prediction. Mathematically a gradient step on the loss. Sklearn's modern implementation is `HistGradientBoostingClassifier`.

- **GridSearchCV** — Exhaustively try every combination of a hyperparameter grid, cross-validating each. Returns the best combination.

- **HistGradientBoosting** — Sklearn's histogram-based gradient boosting. Bins continuous features into ~255 buckets so split-finding is fast. Competitive with XGBoost on many problems, no extra install needed.

- **Hyperparameter** — A setting you choose BEFORE training (e.g., `max_depth=10`). Distinct from a *parameter*, which the algorithm LEARNS during training (e.g., the threshold values for each split).

- **`learning_rate`** — In gradient boosting, how much each new tree contributes. Smaller learning rates need more trees but generalise better. Standard values: 0.01–0.1.

- **`max_depth`** — The maximum number of splits from root to leaf in any tree. Lower = shallower trees = lower variance.

- **`max_iter`** — In boosting, the maximum number of trees to add. Pair with `learning_rate`. Use `early_stopping=True` to auto-pick.

- **`min_samples_leaf`** — Minimum number of training samples in a leaf. Higher = less variance, less overfitting. Common values: 1, 5, 20.

- **`n_estimators`** — In Random Forest, the number of trees to grow. More = more stable predictions but slower training. Diminishing returns past 200–500.

- **`n_jobs`** — How many CPU cores to use. `n_jobs=-1` uses all available cores. Set this for any tree-based or grid-search code unless you have a reason not to.

- **Out-of-bag (OOB) score** — In Random Forest, an estimate of test performance computed from the ~37% of bootstrap-unsampled rows per tree. Free cross-validation. Enable with `oob_score=True`.

- **RandomizedSearchCV** — Sample N random hyperparameter combinations from a distribution, cross-validate each. Cheaper than GridSearchCV when the grid is large.

- **Random Forest** — Bagging on decision trees with feature subsampling at each split. Robust, easy to use, the default first try for tabular data.
