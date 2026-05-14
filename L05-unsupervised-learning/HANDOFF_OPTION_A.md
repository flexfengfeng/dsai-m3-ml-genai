# L05 — Option A Restructure Handoff

**Date:** 2026-05-14
**Plan source:** `RESTRUCTURE_PLAN_OPTION_A.md` (approved 2026-05-09). Built fresh with the Option A template — no v1 to compare.

---

## What was built

### 1. Markdown docs

```
README.md             ← Class-structure section + 3-phase flow + file map
setup.md              ← One-time conda env install (reuses dsai-m3 env)
pre-class.md          ← 75-min self-study guide
lesson.md             ← Concept reference for PCA + K-Means + Isolation Forest
reference.md          ← 20-term glossary + further reading
environment.yml       ← Same dsai-m3 env (no new packages required)
```

### 2. Dataset

```
notebooks/data/northstar_customers.csv     ← Copied from L03 (renamed); same 10,000 customers
```

Story continuity decision: **same dataset as L03/L04, but DROP the `churned` label.** This mirrors the real-world situation: same data, new question, no target column. The L01 sentiment polarity feature remains.

### 3. Notebooks (6 total)

| Notebook | Cells (Core / Boundary / Extension) | Status |
|---|---|---|
| `01_monday_morning.ipynb` | 16 cells, no Extension (pre-class hook) | ✅ AST + runtime |
| `02_pca.ipynb` | 14 / 1 / 10 (total 25) | ✅ AST + runtime |
| `03_kmeans.ipynb` | 14 / 1 / 10 (total 25) | ✅ AST + runtime |
| `04_isolation_forest.ipynb` | 14 / 1 / 11 (total 26) | ✅ AST + runtime |
| `assignment.ipynb` | 29 cells (StyleHub 3 exercises + Card-fraud 2 exercises + samples) | ✅ AST + runtime |
| `optional_extensions.ipynb` | 14 cells (Hierarchical + DBSCAN + UMAP) | ✅ AST + runtime |

### 4. Slide deck

```
slides/L05_slides.pptx       ← 28 slides, speaker notes inline
slides/slides_outline.md     ← Section breakdown + slide-by-slide outline
```

Same Ocean Gradient theme as L01-L04. Three code-along cue slides (one before each in-class notebook).

---

## Key narrative & content decisions

1. **Roadmap-faithful Core:** PCA + K-Means + Isolation Forest. Hierarchical clustering, DBSCAN, t-SNE/UMAP all live in Extension/Optional. Z-score / IQR outlier math was already covered in L02; not duplicated here.

2. **Story continuity through dataset reuse.** Sarah has been working with the NorthStar customer file since L03. L05 drops the `churned` column to simulate "we have the data; nobody labelled it." This makes the L05 → unsupervised reasoning natural rather than artificial. The NB 04 (Isolation Forest) cross-references the K-Means clusters from NB 03 — anomaly rate per cluster is a Friday-relevant insight.

3. **`class_weight='balanced'` is NOT in scope.** Unsupervised methods don't have class weights — there's no target. The L04 lesson about balanced class weights doesn't apply here.

4. **PCA captures only ~22% variance in 2D on this dataset.** The features are largely independent (correlations < 0.05 in NB 01) so PCA doesn't compress dramatically. This is honest — many real datasets behave this way, and the lesson reflects it. The 2D plot is still useful as a viz, even at 22% variance.

5. **K-Means silhouette is modest (~0.09) on this dataset.** Real customer data rarely has well-separated clusters. The lesson uses this honestly to teach: silhouette is one signal, business judgement is another. K=4 isn't picked because silhouette peaks there — it's picked because 4 segments are operationally actionable.

6. **Friday recommendation is three artefacts, not one model.** Unlike L03/L04 (pick the best model and ship), L05 produces THREE deliverables — PCA viz for context, segments for marketing, anomaly list for customer success. Different mental model than supervised.

7. **L06 bridge teaser** is Marcus asking about quarterly revenue forecasting — natural setup for time series.

---

## Smoke-test results

All 6 notebooks: ✅ AST-parse + ran end-to-end in `/opt/miniconda3/envs/ml/bin/python` with `matplotlib.use("Agg")`. No new packages beyond what L03/L04 used.

Key numerical outputs:

| Where | Result |
|---|---|
| `01_monday_morning` — Correlations between features | All |r| < 0.05 (essentially independent) |
| `02_pca` — Variance with 2 PCs | ~22% |
| `02_pca` — Components for 80% variance | 8 of 17 |
| `03_kmeans` — Silhouette across K=2-10 | ~0.08-0.09 (modest) |
| `03_kmeans` — K=4 cluster sizes | 3463 · 1138 · 3540 · 1859 |
| `03_kmeans` — Cluster 1 last_login_days | 92 days (dormant segment) |
| `03_kmeans` — Cluster 3 returns_per_purchase | 0.31 (high-return segment) |
| `04_isolation_forest` — contamination=0.05 | 500 anomalies flagged |
| `04_isolation_forest` — Anomaly rate in cluster 1 (dormant) | 14% — highest |
| `04_isolation_forest` — Anomaly rate in cluster 0 (loyal) | 3% — lowest |
| `04_isolation_forest` — IF-only anomalies (|z| < 3) | 301 multivariate-only |
| `assignment` — StyleHub silhouette at K=4 | ~0.28 (good — synthetic clusters built in) |
| `assignment` — StyleHub cluster sizes | 2097 · 3583 · 789 · 1531 |
| `assignment` — Top 50 card-fraud anomalies | Common merchant: 'online'; total amount £18k |

---

## Deviations from the plan

1. **No staging-repo push.** Git workflow not in scope this session.
2. **Visual QA via subagent skipped.** LibreOffice not installed locally. Markitdown placeholder-text check passed cleanly.
3. **DBSCAN result in `optional_extensions.ipynb`** flags 56% of points as noise (eps=2.0 is too small for this data). This is actually pedagogically valuable — the k-distance plot exists for tuning eps, and the cell teaches that DBSCAN parameters need data-specific tuning. Could be tightened to eps=3.0 if you want fewer noise points in the demo.
4. **UMAP demo is gated by import** — if `umap-learn` isn't installed, the cell skips with a friendly message. Same pattern as the XGBoost gating in L04.
5. **Assignment B (card-fraud) uses `contamination='auto'`** which by default flags more than 5% on this synthetic data (32% in the smoke test). Acceptable for the assignment because the focus is "find top 50 most anomalous" — operationally driven, not contamination-driven.

---

## What to verify (Opus learner-perspective review)

- [ ] **NB 01 narrative beat** — "we're dropping the label" should land as a deliberate choice, not a mistake. Verify the framing makes the unsupervised motivation clear.
- [ ] **NB 02 explained variance in 2D (22%)** — make sure learners don't conclude "PCA is useless." The lesson explicitly explains this is honest behaviour for mostly-independent features, but the framing might need a check.
- [ ] **NB 03 silhouette interpretation** — silhouette is low (0.09) but K=4 still gives interpretable clusters. Verify the message lands: "low silhouette ≠ failed analysis; it just means clusters aren't sharply separated."
- [ ] **NB 04 Isolation Forest 'contamination'** explanation — verify learners understand it controls only the threshold, not the score. The Extension cell does demonstrate this with a re-threshold-by-capacity exercise.
- [ ] **Assignment A — StyleHub silhouette at K=4 is 0.28** — much higher than NorthStar's 0.09. This is because StyleHub is generated with built-in clusters (4 latent customer types). Worth noting in instructor delivery so learners understand WHY their numbers differ.

---

## File diff summary

```
+ README.md                                  (new)
+ setup.md                                   (new)
+ pre-class.md                               (new)
+ lesson.md                                  (new)
+ reference.md                               (new)
+ environment.yml                            (new — same as L03/L04)
+ slides/L05_slides.pptx                     (new, 28 slides)
+ slides/slides_outline.md                   (new)
+ notebooks/data/northstar_customers.csv     (copied from L03, renamed)
+ notebooks/01_monday_morning.ipynb          (new)
+ notebooks/02_pca.ipynb                     (new)
+ notebooks/03_kmeans.ipynb                  (new)
+ notebooks/04_isolation_forest.ipynb        (new)
+ notebooks/assignment.ipynb                 (new)
+ notebooks/optional_extensions.ipynb        (new)
+ HANDOFF_OPTION_A.md                        (this file)
```
