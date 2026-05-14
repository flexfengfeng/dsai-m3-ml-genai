# Pre-class — L06 Time Series Forecasting

**Time:** about 75 minutes. Do this before the live session.

**Goal:** Build intuition for sequential data + understand the trend/seasonality/residual mental model before class.

---

## What you're walking into

End of L05 Marcus asked Sarah:

> *"Sales are SEASONAL. We need to know quarterly revenue 90 days ahead so ops can plan inventory. Can you forecast?"*

This week Sarah works with `northstar_daily_revenue.csv` — 2 years of daily revenue. The data has:
- **Long-term trend** (NorthStar is growing)
- **Annual seasonality** (Q4 holiday spike)
- **Weekly seasonality** (weekends > weekdays)
- **Random noise** (some days are just unusual)

She has to disentangle these and forecast the next 90 days.

By Friday she'll have:
1. A decomposition chart showing trend + seasonality + noise
2. A 90-day revenue forecast
3. A comparison of three forecasting methods

---

## Task 1 (~20 min) — Open `01_monday_morning.ipynb`

This notebook loads the daily-revenue file and explores it visually. You'll see the trend, the weekly pattern, and the Q4 holiday lift — all visible to the eye.

**Open** `notebooks/01_monday_morning.ipynb` and **run every cell**.

As you go, jot down:
- What does the overall TREND look like? Growth, decline, or flat?
- What's the strongest visible SEASONALITY — weekly, annual, both?
- Eyeball forecast: what do you expect revenue to be on day t = 800 (about 70 days past the dataset end)?

---

## Task 2 (~25 min) — Watch two videos

### Video 1 — Time Series Decomposition (Hyndman, 6 min)

[https://otexts.com/fpp3/decomposition.html](https://otexts.com/fpp3/decomposition.html)
(Hyndman & Athanasopoulos — *Forecasting: Principles and Practice* — chapter 3. Read sections 3.1 to 3.3, or watch the StatQuest equivalent.)

Why it matters: the **trend + seasonality + residual** decomposition is the mental model that unlocks every forecasting method downstream.

**Mini-exercise:** in your own words, explain why we'd want to SEPARATE a time series into components rather than just modelling it as one signal.

> **Sample answer:** Components have different shapes, different timescales, and different noise profiles. Trend is smooth and slow. Seasonality is repetitive and predictable. Residual is short-burst noise. By separating them we (a) understand each piece individually, (b) can forecast each piece using the right method, and (c) can spot anomalies in the residual that we couldn't see in the raw series.

### Video 2 — Exponential Smoothing intuition (StatQuest, 10 min)

[https://www.youtube.com/watch?v=L8jcrcLwzhk](https://www.youtube.com/watch?v=L8jcrcLwzhk)

Why it matters: ETS / Holt-Winters is the pragmatic baseline every forecaster should know. It's been used for decades and still competitive.

**Mini-exercise:** ETS gives MORE weight to recent observations and LESS weight to older ones. Why is that a sensible default for forecasting?

> **Sample answer:** Recent observations are more relevant to the immediate future. The world changes (NorthStar may have launched a new product line; customer base may have grown). Older observations might be from a regime that no longer applies. Exponentially decaying weights gracefully de-emphasise the past while still using it for stability.

---

## Task 3 (~30 min) — Quick conceptual exercises

### Exercise 1 — Pick the seasonal period

For each scenario, what `seasonal_periods` value would you use?

1. Daily revenue data, weekend > weekday pattern.
2. Hourly electricity demand, with morning + evening peaks.
3. Monthly retail sales, with a December holiday spike.
4. Daily ice-cream sales, with a summer peak each year.

> **Sample answers:**
> 1. `period=7` (weekly cycle on daily data)
> 2. `period=24` (daily cycle on hourly data)
> 3. `period=12` (annual cycle on monthly data)
> 4. `period=365` or `period=52` if weekly aggregated — annual cycle on daily/weekly data

### Exercise 2 — Why k-fold is forbidden

Explain in two sentences why you CANNOT use `KFold(n_splits=5)` on a time series.

> **Sample answer:** k-fold puts data from arbitrary time points in train vs test. Some test points would be EARLIER than some training points — the model gets to "see the future" during training, which never happens in deployment. Use `TimeSeriesSplit` instead, which always puts the test set later than the training set.

### Exercise 3 — Read a residual plot

After STL decomposition, you plot the residual. Three patterns are possible:

- A: Random-looking noise around zero.
- B: A clear sine wave with a different period than the one you specified.
- C: A clear downward drift.

What does each pattern mean?

> **Sample answers:**
> - **A** (random noise): great — your decomposition captured the structure. Whatever's left is genuine randomness.
> - **B** (leftover periodicity): there's a SEASONALITY you didn't model. Either run STL with a different period, or use a multi-seasonal decomposition (MSTL).
> - **C** (drift): the TREND component didn't fit. Trend might be non-linear (quadratic, exponential) when STL assumed linear. Try a different decomposition or smooth the trend with a longer window.

---

## Active-engagement tips

- **Treat ETS as your baseline.** If a fancier model can't beat ETS by a meaningful margin, ship ETS — simpler is better.
- **Always reserve the most recent time period as your test set.** That's the closest analogue to what the model will see in deployment.

---

## Bring to the session

1. ✅ Run `01_monday_morning.ipynb` end-to-end
2. ✅ Watch / read the decomposition material + the ETS intro
3. ✅ Your three exercise answers ready
4. ✅ One question that didn't click

See you Tuesday morning at Sarah's desk.
