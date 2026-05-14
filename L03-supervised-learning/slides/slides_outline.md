# L03 Slide Deck — Outline

**File:** `L03_slides.pptx`
**Total slides:** 28
**Total lecture time:** ~90 minutes (slides) + 90 minutes (three in-notebook code-alongs)
**Theme:** Ocean Gradient — same as L01/L02 for narrative continuity
**Every slide has speaker notes ≥ 2 sentences.**

---

## Section breakdown

| # | Section | Slides | Time | Notes |
|---|---------|--------|------|-------|
| 1 | Title + L02 recap + Marcus's brief | 4 | 8 min | Connect to L02; preview Sarah's dataset + 3 themes |
| 2 | Preprocessing | 5 | 12 min | Why · scaling · missing values · encoding · Pipeline + ColumnTransformer |
| 3 | The leakage rule + Code-along cue | 2 | 32 min | Split-first rule, then 30-min hands-on `02_preprocessing.ipynb` |
| 4 | Training (logistic regression + coefficients) | 3 | 8 min | Sigmoid · interpretation · train/test split |
| 5 | k-fold cross-validation + Code-along cue | 2 | 32 min | 5-fold diagram, then 30-min hands-on `03_train_validate.ipynb` |
| 6 | Metrics (the punchline) | 4 | 12 min | Accuracy lies · confusion matrix · precision/recall · F1 |
| 7 | Threshold choice + sweep curve + Code-along cue | 3 | 32 min | Tradeoff triangle, sweep visualisation, then 30-min `04_metrics_threshold.ipynb` |
| 8 | Friday recommendation + L04 bridge + assignment + Q&A | 5 | 10 min | Wrap-up, the L04 hook (trees / ensembles), assignment preview |

**Total:** 28 slides · ~90 min slide time · ~90 min in-notebook hands-on

---

## Slide-by-slide

| # | Title | Type |
|---|-------|------|
| 1 | L03 — Supervised Learning Foundations | Title (dark) |
| 2 | Where We Left Off | Recap |
| 3 | The NorthStar Churn Dataset | Data preview |
| 4 | The Three Things This Week Is About | Concept |
| 5 | Preprocessing — Three Problems | Concept |
| 6 | Numerical Scaling | Formula + example |
| 7 | Missing Values | Three options |
| 8 | Categorical Encoding | Label vs One-hot |
| 9 | sklearn Pipeline + ColumnTransformer | Code block |
| 10 | The #1 Rule of Preprocessing | Wrong vs Right code |
| 11 | 🟢 Code-Along: 02_preprocessing.ipynb | Cue (dark) |
| 12 | Logistic Regression | Formula + reasons |
| 13 | Reading the Coefficients | Bar chart |
| 14 | Train/Test Split | Diagram + steps |
| 15 | k-Fold Cross-Validation | 5-fold visual + actual scores |
| 16 | 🟢 Code-Along: 03_train_validate.ipynb | Cue (dark) |
| 17 | Accuracy Is a Lie | Big-number callouts |
| 18 | The Confusion Matrix | 2×2 grid with actual numbers |
| 19 | Precision vs Recall | Formulas + tradeoff table |
| 20 | F1 Score | Formula + threshold-comparison table |
| 21 | The Threshold Choice | Tradeoff triangle |
| 22 | Threshold Sweep — The Picture | PR-curve sketch |
| 23 | 🟢 Code-Along: 04_metrics_threshold.ipynb | Cue (dark) |
| 24 | Friday — Sarah's One-Pager to Marcus | Wrap table |
| 25 | Bridge to Lesson 4 | Bridge (dark) |
| 26 | Assignment Preview | Two-column |
| 27 | Q&A — and What's Next | Closing |
| 28 | L03 — Recap | Closing (dark) |

---

## Design conventions (consistent with L01/L02)

- **Dark slides:** title (1), three code-along cues (11, 16, 23), L04 bridge (25), final recap (28)
- **Content slides:** white background, teal top bar, NAVY 30pt bold title, slide-number footer
- **Code blocks:** Consolas on midnight `#21295C` background
- **Stats:** large Trebuchet MS numbers (44-60pt)

## Acceptance check (run before delivery)

- [x] 28 slides (matches plan target)
- [x] Every slide has speaker notes of 2+ sentences
- [x] All three code-along cues clearly labelled and timed
- [x] No leftover placeholder text (verified with `markitdown | grep -iE "lorem|xxxx"`)
- [x] File opens cleanly in PowerPoint / Keynote / LibreOffice
- [x] Sarah-week narrative continuity preserved across L01 → L02 → L03 → L04
