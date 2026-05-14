# Lesson 6 — Time Series Forecasting
*Concept reference. Open whenever you want to look up a definition or check a formula used in the notebooks.*

> **Where Sarah is.** Marcus's brief from end-of-L05: *"Sales are seasonal. Can you forecast next quarter's revenue?"* Sarah opens `northstar_daily_revenue.csv` — 2 years of daily revenue, 731 rows. By Friday she has to show a 90-day forecast with honest error bars and a recommendation on which method to put into production.

---

## The Big Picture — what makes time series different

Every supervised lesson before this assumed rows are INDEPENDENT. You shuffle them, k-fold cross-validate, and the order doesn't matter. Time series breaks that assumption:

- **Today's value depends on yesterday's.** And last week's. And last year's.
- **The future has not happened yet.** You can never use data from after the prediction date.
- **Random k-fold cross-validation is forbidden.** Information from the future would leak into the past.

The toolkit reflects this: special decomposition methods, special models that respect ordering, and special cross-validation strategies.

The three things you'll be working with:

1. **Trend** — long-term direction (growth, decline, flat)
2. **Seasonality** — repeating cycles (annual, monthly, weekly, daily)
3. **Residual** — the noise + everything else

---

## Part 1 — STL Decomposition

### What it does

**STL = Seasonal-Trend decomposition using Loess.** Given a time series, it separates it into three additive components:

```
y(t)  =  trend(t)  +  seasonal(t)  +  residual(t)
```

You give STL the data and the seasonal period; it gives back the three components.

### When to use it

- **For exploration.** Before forecasting, decompose to UNDERSTAND the data. "Is there a real trend or is it just noise that looks like a trend?"
- **For diagnosis.** A residual that's still oscillating means you missed a seasonality. Iterate.
- **As a forecasting tool itself.** Forecast trend and seasonal separately; recombine. (Less common in practice — ETS and ML methods are usually better.)

### Mechanics in statsmodels

```python
from statsmodels.tsa.seasonal import STL

df_series = df.set_index("date")["revenue_gbp"]   # date-indexed Series

stl = STL(df_series, period=7, robust=True).fit()  # period=7 for weekly seasonality

stl.trend, stl.seasonal, stl.resid    # three numpy arrays
stl.plot()    # standard 4-panel diagnostic plot
```

Key parameter:
- **`period`** — the number of observations in one seasonal cycle. Daily data with weekly seasonality → 7. Monthly data with annual seasonality → 12. Hourly data with daily seasonality → 24.

For series with multiple seasonalities (NorthStar has BOTH weekly AND annual), STL can only handle one at a time. We use **weekly** because it's the strongest at daily granularity. Annual gets captured via lag features in the ML forecast.

### Reading the output

- **Trend** — should be smooth. If it's noisy, you may be over-smoothing. Adjust `trend` parameter or use `robust=True`.
- **Seasonal** — should be repeating. The amplitude tells you how strong the weekly cycle is.
- **Residual** — should look like white noise (no patterns). If you see remaining oscillation, you're missing a seasonality.

### Quick Check — Decomposition

**Q1.** Your STL residual has a clear monthly oscillation. What does that mean?

*Sample answer:* STL only modelled the seasonality you specified in `period`. A leftover monthly pattern means there's monthly seasonality the model didn't capture. Run STL with a longer period (e.g., 30) or use a different decomposition (e.g., MSTL for multiple seasonalities).

**Q2.** Why does STL plot four panels, not three?

*Sample answer:* The fourth panel is the original series. STL plots: original, trend, seasonal, residual — so you can visually verify the additive decomposition (trend + seasonal + residual ≈ original).

**Q3.** Should you use STL on log-transformed data?

*Sample answer:* If the seasonality grows with the level (multiplicative seasonality), yes. Log-transform first, then decompose, then exponentiate the forecast. STL itself assumes additive components.

---

## Part 2 — Classical Forecasting

### Baseline methods — always start here

Every forecasting project should start with **at least one trivial baseline**. If your fancy ML model can't beat the trivial baseline, your fancy ML model isn't helping.

**Naive forecast** — predict the most recent observed value for everything.
```python
y_naive = [y_train[-1]] * forecast_horizon
```

**Seasonal Naive** — predict the value from the SAME point one season ago.
```python
y_seasonal_naive = y_train[-period:].tolist() * (forecast_horizon // period + 1)
```

For NorthStar daily data with weekly seasonality, seasonal naive uses last Monday's revenue to forecast next Monday's. Despite its simplicity, it's a strong baseline on seasonal data.

### Exponential Smoothing (ETS)

ETS = **E**rror, **T**rend, **S**easonality. The pragmatic forecasting model everyone should know.

**The idea**: every forecast is a weighted average of:
- Recent observations (more weight on recent ones)
- An estimated trend component
- An estimated seasonal component

**The three flavours:**
| Method | What it models |
|---|---|
| `SimpleExpSmoothing` | Level only (no trend, no seasonality) |
| `Holt` | Level + trend |
| `ExponentialSmoothing` (a.k.a. Holt-Winters) | Level + trend + seasonality |

### Mechanics in statsmodels

```python
from statsmodels.tsa.holtwinters import ExponentialSmoothing

model = ExponentialSmoothing(
    y_train,
    trend="add",            # additive trend
    seasonal="add",         # additive seasonality
    seasonal_periods=7,     # weekly cycle for daily data
).fit()

forecast = model.forecast(steps=90)   # 90-day forecast
```

Key choices:
- **`trend` / `seasonal`** — "add" for additive (most common); "mul" for multiplicative when the seasonal swing scales with the level
- **`seasonal_periods`** — the seasonal cycle length, same logic as STL's `period`

### Forecast evaluation metrics

For continuous forecasts, three standard metrics:

| Metric | Formula | When |
|---|---|---|
| **MAE** | mean of \|actual − predicted\| | When all errors equally bad |
| **RMSE** | sqrt(mean of (actual − predicted)²) | When large errors are disproportionately bad |
| **MAPE** | mean of \|actual − predicted\| / \|actual\| × 100% | When percentage-error is more intuitive than absolute |

**MAPE is the favourite for business reporting** — but watch out: if actual values can be near zero, MAPE explodes.

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error
import numpy as np

mae  = mean_absolute_error(y_true, y_pred)
rmse = np.sqrt(mean_squared_error(y_true, y_pred))
mape = np.mean(np.abs((y_true - y_pred) / y_true)) * 100
```

### Time-Series Cross-Validation

You CANNOT use random k-fold on time series — it would leak future information into training. Instead use **rolling-origin** or **expanding-window** validation.

```python
from sklearn.model_selection import TimeSeriesSplit

tscv = TimeSeriesSplit(n_splits=5, test_size=30)   # 5 splits, 30-day test windows
for train_idx, test_idx in tscv.split(y):
    train, test = y[train_idx], y[test_idx]
    # train, predict, evaluate
```

Each fold's test set is LATER than its train set. The fifth fold uses the most data and most recent test window — that's your most realistic estimate of forecast performance.

### Quick Check — Classical forecasting

**Q1.** Seasonal Naive beats your fancy ETS model on MAPE. What's happening?

*Sample answer:* Your data is dominated by stable seasonality and the trend is weak. Naive captures the seasonality perfectly because it just looks back one cycle. ETS adds parameters (trend, smoothing) but the parameters didn't help on this data. Sometimes the simple baseline wins — that's a feature, not a bug.

**Q2.** Why not just k-fold cross-validate the forecast model?

*Sample answer:* k-fold would put some future training data in earlier test folds — leaking information that wouldn't be available in deployment. The model would look better in your notebook than in production. Use `TimeSeriesSplit` instead.

**Q3.** ETS converged poorly and gave a warning. Should you trust the forecast?

*Sample answer:* Sometimes. Convergence warnings often mean the data is too short, too noisy, or has structural breaks the model can't fit. Either: (a) get more data, (b) use a different model (LightGBM with lags), or (c) accept the warning if the held-out forecast is still good.

---

## Part 3 — ML-Based Forecasting (Lag Features + GB)

### The modern industry default

Classical methods (ETS, ARIMA) treat time series as a SPECIAL DATA TYPE. The modern approach reframes it: **turn the time series into a tabular regression problem and use a normal ML model.**

The trick: create LAG FEATURES.

### Lag features

For each row in the time series, add columns for past values:

| date       | revenue | lag_1 | lag_7 | lag_30 | day_of_week | month |
|------------|---------|-------|-------|--------|-------------|-------|
| 2024-01-10 | 9200    | 8800  | 9100  | 8500   | 2           | 1     |
| 2024-01-11 | 9400    | 9200  | 9300  | 8600   | 3           | 1     |
| ...        | ...     | ...   | ...   | ...    | ...         | ...   |

`lag_1` is yesterday's revenue. `lag_7` is the same day last week. `lag_30` is roughly a month ago. The model learns: "given yesterday's revenue, same-day-last-week's revenue, and the day of week, predict today's revenue."

```python
df["lag_1"]  = df["revenue_gbp"].shift(1)
df["lag_7"]  = df["revenue_gbp"].shift(7)
df["lag_30"] = df["revenue_gbp"].shift(30)
df["dayofweek"] = df["date"].dt.dayofweek
df["month"]     = df["date"].dt.month
```

Drop the first 30 rows (where lags are NaN), then train any regression model:

```python
from sklearn.ensemble import HistGradientBoostingRegressor

X = df.drop(columns=["revenue_gbp", "date"]).dropna()
y = df.loc[X.index, "revenue_gbp"]

model = HistGradientBoostingRegressor(max_iter=200, learning_rate=0.05, random_state=42)
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

### Why this often beats classical methods

- **Handles multiple seasonalities** for free — just add lag_7 AND lag_365
- **Handles exogenous variables** for free — promotions, weather, holidays = extra columns
- **No assumption of stationarity** or specific time-series structure
- **Same code you already know** — sklearn Pipeline / GridSearchCV / etc

### The trap — recursive vs direct forecasting

**One-step prediction:** "predict day t+1 given data up to day t." Easy — your lag_1 is known.

**Multi-step prediction (90 days ahead):** harder. To predict day t+90, you need lag_1 for day t+90 — which is day t+89's revenue — which you haven't observed yet.

Two strategies:
- **Recursive forecasting:** predict day t+1, plug it into the lag for day t+2, repeat. Errors compound.
- **Direct forecasting:** train SEPARATE models for each horizon (1-day-ahead, 2-day-ahead, ..., 90-day-ahead). More work but errors don't compound.

For the L06 notebook we use recursive (it's simpler). The direct approach lives in the Extension.

### Quick Check — ML-based forecasting

**Q1.** Your GB forecast has RMSE = £500 in training and RMSE = £2000 on a 30-day held-out test. What's happening?

*Sample answer:* Overfitting OR the test period has a different regime (e.g., trained on regular months but tested through a holiday season). Look at the residuals on test — if they have a systematic pattern (consistently low or biased), the model isn't generalising. Try: more regularisation (smaller max_iter), more lag features, or check if the test period is genuinely different from training.

**Q2.** Should you scale lag features before feeding them to HistGradientBoosting?

*Sample answer:* Trees don't care about scale. You don't need to scale for tree-based models. (For neural networks, yes.)

**Q3.** Your ML forecaster includes `lag_1`, `lag_7`, `lag_30`. Should you add `lag_2`, `lag_3`, ...?

*Sample answer:* Probably not. lag_1 already captures recent state. lag_2 and lag_3 are very correlated with lag_1 (lag-1 autocorrelation is high). They add columns without much new information. Stick with biologically meaningful lags: 1 (yesterday), 7 (last week), 30 (last month), 365 (last year). Add more only if you have data-specific reason.

---

## Friday — what Sarah presents

By Friday afternoon Sarah has three deliverables:

1. **A decomposition chart** — trend + weekly seasonality + residual, showing what's predictable and what's noise
2. **A 90-day forecast** with confidence band — based on the winning model
3. **A comparison table** — Naive / Seasonal Naive / ETS / ML-with-lags, each with MAE / RMSE / MAPE on the held-out test

The winner is usually **ML-with-lags or ETS** — they trade leadership depending on the data. The headline forecast says: "Next quarter's revenue: £X ± £Y (90% confidence)."

Marcus listens, nods, then asks:

> *"Great. Now — what if we want to predict whether a customer who STARTED shopping will COMPLETE checkout? Like, in real time as they shop?"*

That question — **prediction from sequential customer behaviour using neural networks** — is the engine of **L07 (Neural Networks & Deep Learning).**

---

## Glossary

See [`reference.md`](./reference.md) for a 20-term glossary covering time-series decomposition, forecasting, and evaluation language used in this lesson.
