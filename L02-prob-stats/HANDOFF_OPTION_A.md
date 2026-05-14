# L02 — Option A Restructure Handoff

**Date:** 2026-05-14
**Plan source:** `learner-edition/RESTRUCTURE_PLAN_OPTION_A.md` (approved 2026-05-09)
**Trigger:** Instructor feedback that the original v1 notebooks were too dense for a 3-hour class with slides + Q&A + environment troubleshooting.

---

## What was built

### 1. Slide deck — NEW

```
slides/L02_slides.pptx       ← 29 slides, speaker notes inline on every slide
slides/slides_outline.md     ← Slide-by-slide outline with section timings
```

- **Theme:** Ocean Gradient (same palette as L01 for narrative continuity)
- **Narrative arc:** mirrors Sarah's week — Monday (distribution discovery) → Tuesday → Wednesday (84% bracket) → Thursday (apology coupon A/B test) → Friday (three defended numbers)
- **Code-along cues:** 3 separate dark slides naming each notebook and its time budget (`02_distributions` 30 min, `03_confidence_intervals` 30 min, `04_ab_testing` 20 min)
- **L03 bridge:** Final two slides set up Marcus's question — "can you train a model on NorthStar's own data?" — which is the engine of L03

### 2. Notebooks — TRIMMED to Core/Extension format

All three in-class notebooks now have a single 🟡 Extension boundary. Above the line is what runs in class; below the line is for self-study.

| Notebook | Cells (Core / Boundary / Extension) | Status |
|---|---|---|
| `02_distributions.ipynb` | 9 / 1 / 7 (total 17) | ✅ AST-clean + runtime-verified end-to-end. |
| `03_confidence_intervals.ipynb` | 10 / 1 / 6 (total 17) | ✅ AST-clean + runtime-verified end-to-end. |
| `04_ab_testing.ipynb` | 11 / 1 / 3 (total 15) | ✅ AST-clean + runtime-verified end-to-end. |

### 3. README — UPDATED

Added a `## How class time is structured (~3 hrs)` section near the top. File map updated to include the new `slides/` folder.

---

## Notebook trim decisions — what moved to Extension

### `02_distributions.ipynb`

| Moved to Extension | Why |
|---|---|
| Z-score concept + compute code + outlier viz code | Plan explicitly listed "Z-score outlier deep-dive cells" as below-the-line. The headline reveal — that Sarah's polarity is right-skewed — stays in Core. |
| Z-score "What do you notice?" reflection | Moved alongside the Z-score deep-dive, in its proper position (previously misplaced at the END of the notebook). |
| Heights vs Incomes contrast section (markdown + code + reflection) | Bonus comparison that drives home mean vs median; useful as self-study, not load-bearing for the Tuesday narrative. |
| OLD combined summary | Replaced with a NEW slim summary in Core that omits the Z-score row (since Z-scores are now Extension). |

**Structural fix during trim:** the original notebook had a misplaced Z-score reflection at position 16 (after the Heights comparison), separated from the Z-score code by 4 unrelated cells. Now correctly adjacent to the Z-score code in Extension.

### `03_confidence_intervals.ipynb`

| Moved to Extension | Why |
|---|---|
| CLT concept + dice demonstration code | Critical theoretical foundation, but learners can grasp CI mechanics in Core without the CLT proof. The reflection cell briefly names CLT as the reason the histogram is bell-shaped. |
| CLT "What do you notice?" reflection (dice rolls) | Moves alongside the CLT code. |
| CI "what do you notice?" reflection (mis-reading warning) | The mis-reading warning is critical content; kept a one-line callout in the Core summary table pointing learners to the Extension for the full discussion. |
| 2nd Pause-and-Predict cell (CI prediction) | Plan rule: at most one Pause-Predict in Core. Kept the first one (motivation for the simulation) since it sets up the headline chart. |
| Visual CI on simulation distribution | Secondary plot — the headline chart already exists in Core. |
| Bonus sample-size table | "Square root rule" deep dive — valuable post-class material. |

**Modification:** the CI concept cell was re-authored slightly to remove its embedded Pause-and-Predict block, keeping just the concept + analogy + formula.

### `04_ab_testing.ipynb`

| Moved to Extension | Why |
|---|---|
| Step 4 — The Three Most Common P-value Mis-readings (long md cell) | Critical content but a deep dive; the Core summary table includes the three mis-readings as a one-line callout and points to the Extension for the full discussion + worked numerical corrections. |
| Visualisation code (bar chart + CI plot of complaint rates) | Secondary plot; the headline reveal (z-stat + p-value) is the main result in Core. |
| Mis-readings quantified code (the corrected/wrong-reading print statements applied to Sarah's actual numbers) | Companion code to the mis-readings markdown — naturally lives in Extension alongside it. |

**Core kept intact:** the 7-step narrative arc through the experiment design (Step 1 framing → Step 2 simulate → Step 3 z-test → reflection → summary including Friday wrap-up + L03 bridge) survives as one continuous flow.

---

## Deviations from the plan

1. **Slide count is 29, not ~32.** Within ±5 of the §2 target. Two slides were merged where the content was tight; the rest hit the per-section count.

2. **No staging-repo push.** Same reason as L01 — git workflow not in scope this session.

3. **Visual QA via subagent skipped.** Recommend a follow-up pass before classroom use. The deck builds cleanly and opens without errors, but a visual inspection by you (or a subagent) would catch any layout edge-cases.

4. **Shape-based "histograms" rather than embedded matplotlib screenshots.** Used filled rectangles to sketch distribution shapes (normal / right-skewed / left-skewed / Skittles / CLT panels). Faster and themed consistently; PNG screenshots from the actual notebook outputs would be more rigorous if you want to swap them in.

---

## What to verify (Opus learner-perspective review)

- [ ] **`02_distributions.ipynb` Core summary** — verify it reads as a complete thought without referencing the Z-score table that was moved.
- [ ] **`03_confidence_intervals.ipynb` flow** — the CLT mention in the Core "what do you notice?" reflection (cell 6) should bridge naturally even without the deep-dive that follows in Extension. Check if a learner who skips Extension still understands "why a bell curve emerges."
- [ ] **`04_ab_testing.ipynb` mis-reading callout** — the one-line callout in the Core summary ("⚠️ Three mis-readings to know…") needs to be prominent enough that learners actually open the Extension before quoting a p-value in real work.
- [ ] **Slide narrative continuity** — slides 1 (title), 2 (recap), and 28 (L03 bridge) should feel like the right opening / closing for Sarah's week.
- [ ] **Code-along timer accuracy** — each code-along cue shows a time budget (30/30/20 min). Validate against the actual time it takes learners to work through the trimmed notebooks.

---

## Smoke-test results

All three L02 notebooks: ✅ AST-clean + ran end-to-end in `/opt/miniconda3/envs/ml` with `matplotlib.use("Agg")`. Required scientific stack already in env (numpy, pandas, matplotlib, seaborn, scipy).

Example outputs verified:
- `02_distributions`: Generates the polarity histogram, computes mean/median/skew. Z-score outlier detection in Extension flags ~27 reviews with |Z| > 3.
- `03_confidence_intervals`: 1,000-simulation distribution centred on 0.83 (true accuracy). Sarah's 95% CI = [78.4%, 88.6%]. 94.7% of simulated estimates fall inside the CI (close to the theoretical 95%).
- `04_ab_testing`: Two-proportion z-test on the simulated experiment gives z = −2.335, p = 0.0098 — statistically significant at α = 0.05.

---

## File diff summary (vs. v1)

```
+ slides/L02_slides.pptx                          (new file, 29 slides)
+ slides/slides_outline.md                        (new file)
~ README.md                                       (added "How class time is structured" section + slides/ to file map)
~ notebooks/02_distributions.ipynb               (17 cells, reorganised with Core/Extension boundary)
~ notebooks/03_confidence_intervals.ipynb        (17 cells, reorganised; CI concept cell slimmed)
~ notebooks/04_ab_testing.ipynb                   (15 cells, reorganised; mis-readings moved below)
+ HANDOFF_OPTION_A.md                             (this file)
```
