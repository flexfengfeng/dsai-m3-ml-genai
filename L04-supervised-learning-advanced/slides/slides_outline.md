# L04 Slide Deck — Outline

**File:** `L04_slides.pptx`
**Total slides:** 28
**Total lecture time:** ~90 minutes (slides) + 90 minutes (three in-notebook code-alongs)
**Theme:** Ocean Gradient — same as L01–L03 for narrative continuity
**Every slide has speaker notes ≥ 2 sentences.**

---

## Section breakdown

| # | Section | Slides | Time | Notes |
|---|---------|--------|------|-------|
| 1 | Title + L03 recap + Marcus's brief | 2 | 5 min | Connect to L03; preview this week's 3-algorithm contest |
| 2 | Why trees + how one tree fails | 2 | 6 min | Non-linear interactions for free; overfitting/underfitting demo |
| 3 | Bagging + Random Forest mechanics | 3 | 9 min | Bootstrap sampling, feature subsampling, sklearn code |
| 4 | class_weight='balanced' + feature importance | 2 | 6 min | The unlock on imbalanced data + interpretability output |
| 5 | Code-along NB 02 (Random Forest) | 1 | 30 min | Hands-on `02_decision_tree_to_forest.ipynb` |
| 6 | Boosting + Gradient Boosting mechanics | 2 | 8 min | Sequential error correction + HistGradientBoostingClassifier |
| 7 | learning_rate × max_iter + early stopping | 1 | 5 min | The coupled hyperparameter pair |
| 8 | Code-along NB 03 (Gradient Boosting) | 1 | 30 min | Hands-on `03_gradient_boosting.ipynb` |
| 9 | Hyperparameter tuning + Grid vs Random | 3 | 9 min | Why tune; GridSearchCV; RandomizedSearchCV |
| 10 | Code-along NB 04 (Tuning + comparison) | 1 | 30 min | Hands-on `04_tuning_and_comparison.ipynb` |
| 11 | Friday recommendation + L05 bridge | 2 | 8 min | Comparison table + Marcus's new question |
| 12 | Assignment preview + Q&A | 2 | 7 min | Kaggle competition + banking fraud |
| 13 | Recap section (confusion matrices, stats, 3 lessons, journey, Kaggle context, interpretability, closing) | 6 | optional / cushion | Padding to ~28 slides per L01/L02/L03 norm |

**Total:** 28 slides · ~90 min slide time · ~90 min in-notebook hands-on

---

## Slide-by-slide

| # | Title | Type |
|---|-------|------|
| 1 | L04 — Trees & Ensembles | Title (dark) |
| 2 | Where We Left Off | Recap |
| 3 | Why Trees Dominate Tabular ML | Concept |
| 4 | One Tree Isn't Enough | Failure demo |
| 5 | Bagging — The Random Forest Idea | Concept |
| 6 | Random Forest in sklearn | Code block |
| 7 | class_weight='balanced' Is The Unlock | Comparison |
| 8 | Feature Importance | Bar chart |
| 9 | 🟢 Code-Along: 02_decision_tree_to_forest.ipynb | Cue (dark) |
| 10 | Boosting — Sequential Error Correction | Concept |
| 11 | HistGradientBoostingClassifier | Code block |
| 12 | The learning_rate × max_iter Tradeoff | Comparison table |
| 13 | 🟢 Code-Along: 03_gradient_boosting.ipynb | Cue (dark) |
| 14 | Why Tune? | Stat callouts |
| 15 | GridSearchCV — Exhaustive Search | Code block |
| 16 | RandomizedSearchCV — Cheaper for Big Grids | Comparison |
| 17 | 🟢 Code-Along: 04_tuning_and_comparison.ipynb | Cue (dark) |
| 18 | Friday — The Final Comparison | Wrap table |
| 19 | Bridge to Lesson 5 | Bridge (dark) |
| 20 | Assignment Preview — Kaggle Competition | Two-column |
| 21 | Q&A and What's Next | Closing |
| 22 | Confusion Matrices — Side By Side | Comparison |
| 23 | Performance Recap | Stat callouts |
| 24 | Three Things to Remember | Concept |
| 25 | L04 — Recap (Sarah's journey) | Closing (dark) |
| 26 | Kaggle Context | Concept |
| 27 | Interpretability vs Accuracy | Spectrum |
| 28 | See You in L05 | Closing (dark) |

---

## Design conventions (consistent with L01/L02/L03)

- **Dark slides:** title (1), three code-along cues (9, 13, 17), L05 bridge (19), Sarah's journey (25), closing (28)
- **Content slides:** white background, teal top bar, NAVY 30pt bold title, slide-number footer
- **Code blocks:** Consolas on midnight `#21295C` background
- **Stats:** large Trebuchet MS numbers (28–48pt)
- **Bar charts:** drawn programmatically with python-pptx rectangles

## Acceptance check

- [x] 28 slides (matches plan target)
- [x] Every slide has speaker notes of 2+ sentences
- [x] Three code-along cues clearly labelled and timed
- [x] No leftover placeholder text (verified with `markitdown | grep -iE "lorem|xxxx"`)
- [x] File opens cleanly in PowerPoint / Keynote / LibreOffice
- [x] Sarah-week narrative continuity preserved across L01 → L02 → L03 → L04 → L05 (preview)
