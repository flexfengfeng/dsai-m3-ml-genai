# L06 — Option A Restructure Handoff

**Date:** 2026-05-14
**Plan source:** `RESTRUCTURE_PLAN_OPTION_A.md` (approved 2026-05-09). Built fresh with the Option A template — no v1 to compare.

---

## What was built

### 1. Markdown docs

```
README.md             ← Class-structure section + 3-phase flow + file map
setup.md              ← One-time conda env install (reuses dsai-m3 env)
pre-class.md          ← 75-min self-study guide
lesson.md             ← Concept reference for decomposition + classical + ML forecasting
reference.md          ← 20-term glossary + further reading
environment.yml       ← Same dsai-m3 env (no new packages)
```

### 2. Dataset

```
notebooks/data/northstar_daily_revenue.csv     ← 731 daily revenue records (2024-01-01 → 2025-12-31)
```

Synthetic but realistic: linear trend + annual cycle (Nov-Dec spike) + weekly cycle (weekend lift) + noise. Multi-seasonal so the lesson can show why ML-with-lags beats single-seasonality classical methods.

### 3. Notebooks (6 total)

| Notebook | Cells | Status |
|---|---|---|
| `01_monday_morning.ipynb` | 16 cells, no Extension | ✅ AST + runtime |
| `02_decomposition.ipynb` | Core 19 + boundary + Ext 11 (total 31) | ✅ AST + runtime |
| `03_classical_forecasting.ipynb` | Core 18 + boundary + Ext 9 (total 28) | ✅ AST + runtime |
| `04_ml_forecasting.ipynb` | Core 21 + boundary + Ext 9 (total 31) | ✅ AST + runtime |
| `assignment.ipynb` | 33 cells (electricity 4 exercises + coffee shop 2 exercises + samples) | ✅ AST + runtime |
| `optional_extensions.ipynb` | 17 cells (stationarity + ACF/PACF + ARIMA + AutoARIMA + SARIMA + Prophet-gated) | ✅ AST + runtime |

### 4. Slide deck

```
slides/L06_slides.pptx       ← 28 slides, speaker notes inline
slides/slides_outline.md     ← Section breakdown + slide-by-slide outline
```

Same Ocean Gradient theme as L01–L05. Three code-along cue slides (one before each in-class notebook).

---

## Key narrative & content decisions

1. **Roadmap-faithful Core:** STL + ETS + ML-with-lags (the roadmap mentioned "Prophet OR LightGBM with lag features" — I chose LightGBM/HistGradientBoosting for continuity with L04 and to avoid extra dependencies).

2. **Multi-seasonal dataset.** The NorthStar revenue series has BOTH weekly and annual seasonality. This was a deliberate choice to motivate the "ML-with-lags beats single-seasonality classical" narrative. STL with period=7 handles the weekly piece; ML-with-lag_365 picks up the annual piece.

3. **Honest about the holiday-window challenge.** The last 60 days of the test set fall in the Q4 holiday spike. All methods struggle on this single window — including the ML method. The notebooks acknowledge this and lean on **TimeSeriesSplit cross-validation** to give a more representative result (ML-CV-MAE ≈ £500 vs single-window MAE ≈ £2,500). The lesson emphasises: don't trust a single window.

4. **Recursive vs direct forecasting** is explicitly contrasted. NB 04 uses recursive for the headline forecast (deployment-realistic) but the Extension shows direct forecasting (which generally has better long-horizon MAE).

5. **`HistGradientBoostingRegressor`, not LightGBM.** Same reasoning as L04 — sklearn-only, no extra install.

6. **Prophet is gated by import** in `optional_extensions.ipynb`. Same pattern as XGBoost in L04. Silently skips if not installed.

---

## Smoke-test results

All 6 notebooks: ✅ AST-parse + ran end-to-end. No new packages.

Key numerical outputs:

| Where | Result |
|---|---|
| `01_monday_morning` — dataset shape | 731 days × 3 columns |
| `01_monday_morning` — weekend vs weekday | Weekend mean £10,711 vs weekday £9,265 |
| `02_decomposition` — STL residual std | ~£380 (the noise floor) |
| `02_decomposition` — Seasonal Naive on holiday window | MAE £2,589 / MAPE 21.3% (high because of holiday spike) |
| `03_classical_forecasting` — Naive on holiday window | MAE £1,270 / MAPE 10.7% (coincidentally good — last train value near holiday level) |
| `03_classical_forecasting` — ETS-weekly | MAE £2,047 / MAPE 17.0% |
| `03_classical_forecasting` — 90-day ETS forecast for Q1 2026 | ~£1,090,000 total |
| `04_ml_forecasting` — ML one-step on holiday window | MAE £1,460 / MAPE 12.2% |
| `04_ml_forecasting` — ML recursive | MAE £2,552 (errors compound over 60-step horizon) |
| `04_ml_forecasting` — CV MAE (TimeSeriesSplit) | ~£500 — best in class on representative windows |
| `04_ml_forecasting` — Top permutation importance | lag_7 (176), dayofweek (24), lag_1 (16), lag_365 (14) |
| `assignment` — electricity ETS-weekly (7-day) | MAE 366 MWh / MAPE 0.9% |
| `assignment` — coffee-shop ML beats Seasonal Naive | 27.1 vs 43.3 customers (37% improvement) |
| `optional_extensions` — SARIMA(1,1,1)(1,1,1,7) | MAE £2,572 — comparable to ML-recursive |

---

## Deviations from the plan

1. **No staging-repo push.**
2. **Visual QA via subagent skipped** — LibreOffice not available locally. Placeholder-text check clean.
3. **Single-window evaluation surfaces awkward numbers.** On the 60-day holiday-window test, Naive's MAE (£1,270) is actually the lowest by coincidence. The lesson honestly acknowledges this and uses it as the "single windows lie; cross-validate" teaching moment. Cross-validated MAE shows ML clearly winning.
4. **ETS-annual (seasonal_periods=365) fails to fit** with only ~2 years of training data. NB 03 catches the exception and reports it. This is a real-world failure mode worth showing.
5. **Permutation importance ranks `lag_30` slightly negative** — meaning shuffling it actually slightly *improves* test MAE. This is normal noise; the takeaway is that lag_30 is essentially useless for this series. The notebook discusses this.

---

## What to verify (Opus learner-perspective review)

- [ ] **NB 02 STL plot** — verify the residual really does look like noise (no remaining cyclical pattern). If not, the period=7 choice needs revisiting.
- [ ] **NB 03 "Naive wins on this window" framing** — make sure learners understand this is a single-window artefact, not a deep result. The current note explicitly calls this out.
- [ ] **NB 04 single-window vs CV-MAE messaging** — verify the message lands: "single test window can mislead; trust cross-validated metric." This is THE key methodological lesson of L06.
- [ ] **Assignment A4 conditional WHY** — check that the printed "WHY" matches the actual winner (the code branches on which method won).
- [ ] **Sarah's L07 bridge** — does the "predict from sequential customer behaviour" hook feel natural after L06? It should — time-series and sequential behaviour data are siblings.

---

## File diff summary

```
+ README.md                                  (new)
+ setup.md                                   (new)
+ pre-class.md                               (new)
+ lesson.md                                  (new)
+ reference.md                               (new)
+ environment.yml                            (new — same as L05)
+ slides/L06_slides.pptx                     (new, 28 slides)
+ slides/slides_outline.md                   (new)
+ notebooks/data/northstar_daily_revenue.csv (new, 731 rows)
+ notebooks/01_monday_morning.ipynb          (new)
+ notebooks/02_decomposition.ipynb           (new)
+ notebooks/03_classical_forecasting.ipynb   (new)
+ notebooks/04_ml_forecasting.ipynb          (new)
+ notebooks/assignment.ipynb                 (new)
+ notebooks/optional_extensions.ipynb        (new)
+ HANDOFF_OPTION_A.md                        (this file)
```
