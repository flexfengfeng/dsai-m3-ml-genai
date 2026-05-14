# L06 — Further Reading & Glossary

## Further reading

### Videos (free)

- **StatQuest — Time Series Concepts**
  Browse StatQuest's "Time Series" playlist on YouTube for ARIMA, exponential smoothing, and decomposition videos.

- **Forecasting: Principles and Practice (Hyndman & Athanasopoulos)**
  Free online textbook: [https://otexts.com/fpp3/](https://otexts.com/fpp3/)
  The definitive intro to forecasting. Chapters 3 (decomposition), 7 (ETS), 9 (ARIMA) cover what we did + the optional content.

### Interactive / official docs

- **statsmodels — Time Series Analysis**
  [https://www.statsmodels.org/stable/tsa.html](https://www.statsmodels.org/stable/tsa.html)
  STL, ETS, ARIMA, SARIMA, VAR — all here.

- **Prophet (Facebook) docs**
  [https://facebook.github.io/prophet/](https://facebook.github.io/prophet/)
  The popular forecasting library — handles multiple seasonalities + holidays out of the box.

- **scikit-learn — TimeSeriesSplit**
  [https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)

### Books

- **Forecasting: Principles and Practice** (Hyndman & Athanasopoulos) — free, comprehensive, the standard reference.
- **Time Series Analysis** (Hamilton) — classical econometric foundation; only if you want the deep math.

---

## Glossary (20 key terms)

- **ACF (Autocorrelation Function)** — Correlation of a series with a lagged version of itself. Large ACF at lag k means the value k steps ago helps predict the current value. Used to spot seasonality.

- **ARIMA(p, d, q)** — AutoRegressive Integrated Moving Average. A classical forecasting model defined by three integers: p (autoregressive terms), d (differencing order to make the series stationary), q (moving-average terms).

- **AutoARIMA** — Automatic parameter selection for ARIMA. Searches over (p, d, q) combinations and picks the best by AIC. Use it; don't tune p/d/q by hand unless you really want to.

- **Decomposition** — Splitting a time series into trend + seasonality + residual. STL and classical decomposition are the two standard methods.

- **Differencing** — Replacing y[t] with y[t] − y[t−1]. Used to remove trend and make a series stationary before ARIMA modelling.

- **ETS (Error, Trend, Seasonality)** — A family of exponential-smoothing models. Three letters describing how each component is modelled (none, additive, multiplicative).

- **Exponential Smoothing** — A forecasting method that gives more weight to recent observations. Comes in three flavours (simple, Holt, Holt-Winters) depending on whether trend and seasonality are modelled.

- **Forecast horizon** — How many time steps ahead you're predicting. 1-step-ahead is much easier than 90-step-ahead.

- **Lag feature** — A column in your modified dataset containing the target value from N time steps ago (`lag_1`, `lag_7`, ...). The key to turning a time series into a tabular regression problem.

- **MAE (Mean Absolute Error)** — `mean(|y_true − y_pred|)`. Same units as the target. Intuitive.

- **MAPE (Mean Absolute Percentage Error)** — `mean(|y_true − y_pred| / |y_true|) × 100%`. Scale-free. Watch out when y_true can be near zero.

- **Naive forecast** — Predict the last observed value for everything in the forecast horizon. The minimum-viable baseline.

- **PACF (Partial Autocorrelation Function)** — Correlation between y[t] and y[t−k] after removing the effect of intermediate lags. Used to pick ARIMA's p parameter.

- **Recursive forecasting** — To predict t+h, predict t+1 first, then use that prediction to predict t+2, etc. Errors compound.

- **RMSE (Root Mean Squared Error)** — `sqrt(mean((y_true − y_pred)²))`. Penalises large errors more than MAE. Standard for forecasting evaluation.

- **Seasonal Naive forecast** — Predict y[t−period]. For daily data with weekly seasonality, predict same-day-last-week. A strong baseline on seasonal data.

- **Seasonality** — Repeating cycles in a time series. Annual, monthly, weekly, daily — pick the period that matters for your data.

- **STL (Seasonal-Trend decomposition using Loess)** — A robust decomposition method that handles outliers gracefully and allows the seasonal pattern to evolve slowly over time.

- **Stationarity** — A time series whose statistical properties (mean, variance) don't change over time. ARIMA assumes stationarity; differencing is the standard fix.

- **TimeSeriesSplit** — sklearn's cross-validator for time series. Always puts test data later than training data. NEVER use plain KFold on time series.

- **Trend** — The long-term direction of the series (growth, decline, flat). Often captured as a smooth curve.
