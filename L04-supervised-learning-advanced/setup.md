# Setup — L04

You only need to do this once. **If you already set up the `dsai-m3` environment for L01/L02/L03, you can skip directly to [Activate](#activate)** — `scikit-learn` already includes `HistGradientBoostingClassifier`, `RandomForestClassifier`, and `GridSearchCV`.

---

## Create the environment

From the **root of the learner-edition folder**:

```bash
conda env create -f L04-supervised-learning-advanced/environment.yml
```

This takes 10–15 minutes the first time. Same environment works for all 10 lessons.

---

## Activate

```bash
conda activate dsai-m3
```

Verify by running:

```bash
python -c "from sklearn.ensemble import RandomForestClassifier, HistGradientBoostingClassifier; from sklearn.model_selection import GridSearchCV; print('sklearn ready for L04')"
```

You should see:
```
sklearn ready for L04
```

---

## (Optional) XGBoost or LightGBM

The Core L04 content uses `HistGradientBoostingClassifier` from sklearn — fast, no extra install. If you want to try **XGBoost** or **LightGBM** in the Extension section or the Kaggle assignment, add them with pip:

```bash
pip install xgboost lightgbm
```

Both libraries have a sklearn-compatible API (`.fit()` / `.predict()` / `.predict_proba()`) so they slot into existing Pipelines.

---

## Launch the notebooks

From inside `L04-supervised-learning-advanced/`:

```bash
jupyter notebook
```

Open `notebooks/01_monday_morning.ipynb` to begin.

---

## Troubleshooting

- **`FileNotFoundError: northstar_churn.csv`** — run notebooks from inside `L04-supervised-learning-advanced/notebooks/` so the relative path `data/northstar_churn.csv` resolves. The dataset is the same one from L03 (already copied here for self-contained use).
- **GridSearchCV is slow** — it trains many models. Make sure you're using `n_jobs=-1` to parallelise across CPU cores. The Core grids in this lesson are sized to complete in under 2 minutes on a modern laptop.
- **`ImportError: No module named xgboost`** — only needed if you're doing the Extension content. Run `pip install xgboost` and restart the kernel.
