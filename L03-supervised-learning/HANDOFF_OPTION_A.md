# L03 — Option A Restructure Handoff

**Date:** 2026-05-14
**Plan source:** `learner-edition/RESTRUCTURE_PLAN_OPTION_A.md` (approved 2026-05-09), section 6 question 3 — "Apply this template to L03 from the start? **Default: yes.**"

**This lesson was built from scratch using the Option A pattern from day 1** (slim Core notebooks + slide deck + Sarah's narrative), so no pre-existing v1 to compare against.

---

## What was built

### 1. Markdown docs

```
README.md             ← Class-structure section, 3-phase flow, file map
setup.md              ← One-time conda env install (reuses dsai-m3 from L01/L02)
pre-class.md          ← 75-min self-study guide (videos + exercises with sample answers)
lesson.md             ← Concept reference for all three Core topics
reference.md          ← 20-term glossary + further reading
environment.yml       ← Same dsai-m3 env (sklearn was already installed)
```

### 2. Dataset

```
notebooks/data/northstar_churn.csv     ← 10,000 customers, 11 features, 12% churn rate
```

The synthetic dataset is designed to exercise every Core preprocessing step:

| Feature class | Where it shows up | Why it's there |
|---|---|---|
| Skewed numeric | `avg_monthly_spend_gbp` (lognormal, 0–500) | Demonstrates why median imputation beats mean |
| Multi-scale numeric | `age` 18–80 vs `returns_per_purchase` 0–0.5 | Demonstrates why StandardScaler matters |
| Missing-at-random | `last_login_days_ago` (~8% NaN) | Standard median imputation |
| Informative missingness | `avg_review_polarity` (~30% NaN) | Customers who don't review behave differently — Extension covers the `add_indicator=True` flag |
| Multi-class nominal | `region` (6 values) | One-hot encoding example |
| Ordinal | `subscription_tier` (3 values) | Conversation about ordinal vs one-hot |
| Causal target | `churned` (logit driven by tenure, returns, polarity, support tickets, premium tier) | Coefficients in notebook 03 match intuition |
| **L01 callback** | `avg_review_polarity` = sentiment score from L01 | Narrative continuity — Sarah's L01 work becomes a feature for her L03 model |

### 3. Notebooks (6 total)

| Notebook | Cells (Core / Boundary / Extension) | Status |
|---|---|---|
| `01_monday_morning.ipynb` | 16 cells, no Extension (pre-class hook) | ✅ AST + runtime |
| `02_preprocessing.ipynb` | 13 / 1 / 10 (total 24) | ✅ AST + runtime |
| `03_train_validate.ipynb` | 14 / 1 / 9 (total 24) | ✅ AST + runtime |
| `04_metrics_threshold.ipynb` | 14 / 1 / 9 (total 24) | ✅ AST + runtime |
| `assignment.ipynb` | 39 (Lakeside 3-tier + hospital exercises + samples) | ✅ AST + runtime |
| `optional_extensions.ipynb` | 14 (bias-variance + ROC-AUC + learning curves + feature engineering) | ✅ AST + runtime |

Core-section sizes are slightly heavier than the plan target (~10 cells) because the L03 narrative — preprocess → train → evaluate → choose threshold — is irreducibly multi-step. The Core for each notebook still fits inside ~30 minutes of in-class hands-on.

### 4. Slide deck

```
slides/L03_slides.pptx       ← 28 slides, speaker notes inline on every slide
slides/slides_outline.md     ← Section breakdown + slide-by-slide outline
```

- Same Ocean Gradient palette as L01/L02
- Three code-along cue slides (one before each in-class notebook)
- The "Accuracy is a Lie" slide (slide 17) uses Sarah's actual numbers — 0.883 vs baseline 0.880 — to deliver the punchline that makes the threshold-choice notebook land

---

## Key narrative & content decisions

1. **L01 → L02 → L03 continuity baked into the data.** The churn dataset's `avg_review_polarity` column IS the L01 sentiment model output. Story callback is visible in cell 0 of `01_monday_morning.ipynb` and the dataset preview slide (slide 3).

2. **Sarah's week structure mirrors L01/L02:** Monday open the data, Tuesday clean it, Wednesday train, Thursday evaluate + threshold, Friday present to Marcus. Each in-class notebook is one day.

3. **The "accuracy is a lie" reveal** lands in notebook 04 cell 6 (confusion matrix at threshold 0.5 catches only 7 of 239 churners). This is the strongest pedagogical moment in the lesson — accuracy = 0.878, recall = 0.029. The slide deck mirrors this with the "Accuracy Is a Lie" slide directly setting up the threshold-choice content.

4. **Threshold choice is framed as the BUSINESS decision.** The lesson resists giving learners a single "right" threshold — it explicitly trains them to derive it from operational constraints (retention team capacity ~ 200 customers/week → threshold 0.25).

5. **Vehicle model: logistic regression.** Chosen for interpretability + standard baseline. Trees and ensembles are explicitly framed as "L04 content" — Marcus's closing question on slide 25 sets that up.

6. **Cross-validation included in Core** — the L03 roadmap entry lists it as Core, even though `M3_Lesson_Sequence_Proposal.md` had a draft splitting CV across L03/L04. I followed the roadmap.

---

## Smoke-test results

All 6 notebooks passed both AST parse and end-to-end execution in `/opt/miniconda3/envs/ml/bin/python` with `matplotlib.use("Agg")`. Required packages: numpy, pandas, matplotlib, seaborn, scikit-learn — all in `environment.yml`.

Key numerical outputs that learners will see:

| Where | Result |
|---|---|
| `01_monday_morning` — churn rate by tenure | 0–6 mo: 22% · 7–12: 19% · 13–24: 15% · 25–48: 11% · 49–72: 6% |
| `02_preprocessing` — final feature count | 10 → 17 (8 numeric + 6 region + 3 tier) |
| `02_preprocessing` — post-scaling stats for `avg_monthly_spend_gbp` | mean 0.00, std 1.00 (was £69.88 / £55.73) |
| `03_train_validate` — top coefficients | returns_per_purchase +0.57, tenure_months −0.53, polarity −0.35 |
| `03_train_validate` — 5-fold CV accuracy | 0.883 ± 0.002 |
| `04_metrics_threshold` — @ threshold 0.5 | recall 0.029 (7 of 239 churners) |
| `04_metrics_threshold` — @ threshold 0.25 (capacity-based) | recall 0.264 (63 of 239), F1 0.282, 208 flagged |
| `optional_extensions` — AUC | 0.736 |
| `optional_extensions` — bias-variance | degree 1 gap 0.4% → degree 5 gap 3.2% (visible overfitting) |
| `assignment` — Lakeside churn rate | 5.3% (calibrated synthetic) |
| `assignment` — hospital readmission rate | 11.3% (calibrated synthetic with real signal) |

---

## Deviations from the plan

1. **Slide count 28 (within ±5 of the L01/L02 norm).** L01 had 22; L02 had 29; L03 has 28. The plan section 7 says ±5; we're well inside.

2. **Lakeside synthetic churn rate is 5.3%, not the more typical 10–15%.** I tuned the intercept toward the lower end so the assignment scenario gives learners a slightly different challenge than the NorthStar dataset (where the rate is 12%). Both rates are realistic — bank churn is generally lower than e-commerce churn.

3. **No staging-repo push.** Same as L01/L02 — git workflow not in this session's scope. The user can mirror to `gh-staging-l03` whenever ready.

4. **Visual QA via subagent skipped.** LibreOffice not installed locally, so I couldn't render slides to images for layout inspection. Markitdown placeholder-text check passed cleanly. Recommend manual visual review before classroom use.

5. **No `assets/` content yet.** The folder exists but is empty. Adding a course-roadmap infographic or pipeline diagram would be a low-cost polish step.

---

## What to verify (Opus learner-perspective review)

- [ ] **The "accuracy is a lie" reveal lands in notebook 04 cell 6.** Test by reading from scratch — does the surprise hit?
- [ ] **Coefficient interpretation in notebook 03** — the bar chart should make returns_per_purchase visually dominant. Verify the order matches Sarah's intuition from `01_monday_morning.ipynb`.
- [ ] **Pipeline cells in notebook 02 vs 03 vs 04** — three notebooks rebuild the same pipeline (so each runs standalone). The reused boilerplate should feel intentional (a recap), not lazy. Consider whether moving the rebuilds to a small `utils.py` would be worth the loss of self-contained notebook semantics.
- [ ] **Assignment Lakeside scenario** — the 5.3% churn rate makes Tier 1's default-threshold metrics look poor. Verify that learners working through Tier 3 (capacity = 300) get a satisfying "this is what good looks like" moment.
- [ ] **Optional extensions feature engineering AUC lift = 0.000** — the notebook explicitly addresses this ("logistic regression already captures these interactions; trees benefit more"). Verify that explanation is clear enough that learners don't feel they did something wrong.

---

## File diff summary (vs. zero state)

```
+ README.md                                  (new)
+ setup.md                                   (new)
+ pre-class.md                               (new)
+ lesson.md                                  (new)
+ reference.md                               (new)
+ environment.yml                            (new)
+ slides/L03_slides.pptx                     (new, 28 slides)
+ slides/slides_outline.md                   (new)
+ notebooks/data/northstar_churn.csv         (new, 10,000 rows)
+ notebooks/01_monday_morning.ipynb          (new)
+ notebooks/02_preprocessing.ipynb           (new)
+ notebooks/03_train_validate.ipynb          (new)
+ notebooks/04_metrics_threshold.ipynb       (new)
+ notebooks/assignment.ipynb                 (new)
+ notebooks/optional_extensions.ipynb        (new)
+ HANDOFF_OPTION_A.md                        (this file)
```
