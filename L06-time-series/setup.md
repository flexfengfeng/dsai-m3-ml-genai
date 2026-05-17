# L06 — Time Series Forecasting — Setup

Environment setup is at the repo root: ➡ **[../SETUP.md](../SETUP.md)**

If you completed setup for an earlier lesson, your `dsai-m3` environment already has everything L06 needs. Skip ahead.

---

## What's new this lesson

**Dependencies:** `statsmodels` (STL, ETS) + `HistGradientBoostingRegressor` (already in `dsai-m3`). Prophet is optional and gated behind a try-import in `optional_extensions.ipynb`.

**Data:** `notebooks/data/northstar_daily_revenue.csv` (~22 KB, two years of daily revenue).


## Sanity check

Open any notebook in this lesson, pick the `dsai-m3` kernel, run the setup cell. If anything errors, see **Troubleshooting** in [../SETUP.md](../SETUP.md).
