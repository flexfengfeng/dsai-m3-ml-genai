# L03 — Further Reading & Glossary

## Further reading

### Videos (free, beginner-friendly)

- **StatQuest — Logistic Regression, clearly explained**
  [https://www.youtube.com/watch?v=yIYKR4sgzI8](https://www.youtube.com/watch?v=yIYKR4sgzI8)
  Why logistic regression doesn't predict a number directly but a probability between 0 and 1.

- **StatQuest — Bias and Variance**
  [https://www.youtube.com/watch?v=EuBBz3bI-aA](https://www.youtube.com/watch?v=EuBBz3bI-aA)
  Strong intuition for under- and overfitting in 5 minutes.

- **StatQuest — Confusion Matrix, Sensitivity, Specificity**
  [https://www.youtube.com/watch?v=Kdsp6soqA7o](https://www.youtube.com/watch?v=Kdsp6soqA7o)
  The four corners of the confusion matrix, with worked examples.

- **3Blue1Brown — Cross-validation visualisation**
  Look up "k-fold cross-validation" on his channel; the animation makes the splitting strategy click.

### Interactive

- **scikit-learn documentation — Preprocessing**
  [https://scikit-learn.org/stable/modules/preprocessing.html](https://scikit-learn.org/stable/modules/preprocessing.html)
  Every scaler, imputer, and encoder is here. Bookmark this page.

- **scikit-learn — Pipeline tutorial**
  [https://scikit-learn.org/stable/modules/compose.html](https://scikit-learn.org/stable/modules/compose.html)
  The official guide to chaining preprocessing + model into one object.

- **Google Crash Course — Classification thresholds**
  [https://developers.google.com/machine-learning/crash-course/classification/thresholding](https://developers.google.com/machine-learning/crash-course/classification/thresholding)
  Interactive sliders for precision/recall tradeoff. Worth 10 minutes.

### Books

- **Hands-On Machine Learning** by Aurélien Géron — chapters 2 and 3.
  Best practical book for sklearn workflow. Worth buying.

- **An Introduction to Statistical Learning** (free PDF) — chapters 4 and 5.
  More mathematical foundation. Free at [https://www.statlearning.com/](https://www.statlearning.com/)

---

## Glossary (20 key terms)

- **Bias** — Error from a model that is too simple to capture the patterns in the data. High-bias models *underfit*: they get both training and test data wrong in the same way.

- **Class imbalance** — When the target variable is much more common in one class than the other (e.g., 88% non-churners vs 12% churners). Accuracy alone is misleading on imbalanced data; use precision/recall instead.

- **Confusion matrix** — A 2×2 table for binary classifiers showing true positives, false positives, false negatives, and true negatives. The starting point for almost every classification metric.

- **Cross-validation (k-fold)** — Split the training data into k equal pieces. Train k times, each time using k–1 pieces for training and the remaining one for validation. The average of the k validation scores is a more stable estimate of model performance than a single train/test split.

- **F1 score** — The harmonic mean of precision and recall. Useful when you care about both false positives and false negatives. Ranges 0 to 1.

- **False negative (FN)** — The model said NO, but the answer was actually YES. In churn prediction: a customer the model thought was safe, but who actually left.

- **False positive (FP)** — The model said YES, but the answer was actually NO. In churn prediction: a customer the model flagged as a churn risk, but who actually stayed.

- **Feature** — A column in your dataset that the model uses as input. Sometimes called "predictor" or "independent variable".

- **Imputation** — Filling in missing values. Common strategies: mean (numerical), median (numerical with outliers), most frequent (categorical), or "constant" (a sentinel value).

- **Label** — The column you are trying to predict. Sometimes called "target", "outcome", or "dependent variable".

- **Logistic regression** — A classification algorithm that predicts the probability of an outcome (0 to 1) using a linear combination of the features passed through a sigmoid function. Most interpretable classifier; usually the right baseline.

- **One-hot encoding** — Turning a categorical variable into multiple 0/1 columns, one per category. Avoids the trap of teaching the model false ordering between categories.

- **Overfitting** — When a model has learned the training data too well (including its noise) and performs poorly on new data. Sign: training accuracy much higher than test accuracy.

- **Pipeline (sklearn)** — A single object that chains preprocessing steps with a model. Ensures the same transformations applied during training are applied during prediction. Without pipelines, leakage and inconsistency are almost inevitable.

- **Precision** — Of all the items the model labelled positive, what fraction actually were positive? `TP / (TP + FP)`.

- **Recall** — Of all the items that were actually positive, what fraction did the model catch? `TP / (TP + FN)`.

- **Standard scaling (z-score)** — Subtract the mean and divide by the standard deviation. Each feature becomes mean 0, std 1. Required for distance-based and gradient-based models (KNN, logistic regression, neural networks).

- **Threshold** — For probabilistic classifiers like logistic regression, the cut-off above which a prediction is labelled "positive". Default is 0.5, but you can and should tune it for your business objective.

- **Train/test split** — Holding out a fraction of the data (typically 20–30%) before training. The model never sees this data during training, so its performance there is a fair estimate of how it'll do on new data.

- **Variance** — Error from a model that is too sensitive to specific patterns in the training data. High-variance models *overfit*: they perform well on training data but poorly on new data.
