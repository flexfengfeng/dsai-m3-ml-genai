# Setup — L06

Skip if you already have the `dsai-m3` environment from L01–L05. `statsmodels` (STL, ETS) and `scikit-learn` (HistGradientBoostingRegressor) are already in your environment.

---

## Create the environment

From the **root of the learner-edition folder**:

```bash
conda env create -f L06-time-series/environment.yml
```

---

## Activate

```bash
conda activate dsai-m3
```

Verify:

```bash
python -c "from statsmodels.tsa.seasonal import STL; from statsmodels.tsa.holtwinters import ExponentialSmoothing; from sklearn.ensemble import HistGradientBoostingRegressor; print('Ready for L06')"
```

You should see:
```
Ready for L06
```

---

## (Optional) Prophet

If you want to try Prophet in the Extension section:

```bash
pip install prophet
```

Prophet has a heavier dependency footprint than the Core libraries — only install if you'll actually use it.

---

## Launch the notebooks

From inside `L06-time-series/`:

```bash
jupyter notebook
```

Open `notebooks/01_monday_morning.ipynb` to begin.

---

## Troubleshooting

- **`FileNotFoundError: northstar_daily_revenue.csv`** — run notebooks from inside `L06-time-series/notebooks/`.
- **`ImportError: No module named prophet`** — only needed for the Extension content. Run `pip install prophet`.
- **`statsmodels.tools.sm_exceptions.ConvergenceWarning`** — exponential-smoothing models sometimes fail to converge cleanly. Usually harmless; the forecast still works. Set `warnings.filterwarnings("ignore")` if it's noisy.
