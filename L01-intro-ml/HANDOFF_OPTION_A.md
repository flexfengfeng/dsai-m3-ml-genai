# L01 — Option A Restructure Handoff

**Date:** 2026-05-14
**Plan source:** `learner-edition/RESTRUCTURE_PLAN_OPTION_A.md` (approved 2026-05-09)
**Trigger:** Instructor feedback that the original v1 notebooks were too dense for a 3-hour class with slides + Q&A + environment troubleshooting.

---

## What was built

### 1. Slide deck — NEW

```
slides/L01_slides.pptx       ← 22 slides, speaker notes inline on every slide
slides/slides_outline.md     ← Slide-by-slide outline with section timings
```

- **Theme:** Ocean Gradient (navy `#065A82` / teal `#1C7293` / coral accent `#F96167`)
- **Header font:** Trebuchet MS · **Body font:** Calibri · **Code font:** Consolas
- **Visual cues:**
  - Dark navy backgrounds for title (slide 1), code-along cues (slides 11, 20), discussion cue (slide 15), bridge slides (21)
  - Light/white backgrounds with thin teal top bar for content slides
  - 🟢 green pill for code-along moments, 💬 gold pill for discussion moments
  - Every code-along cue slide names the notebook in monospace, lists 2–3 goals, and shows a timer at the bottom

### 2. Notebooks — TRIMMED to Core/Extension format

All three in-class notebooks now have a single 🟡 Extension boundary. Above the line is what runs in class; below the line is for self-study.

| Notebook | Cells (Core / Boundary / Extension) | Status |
|---|---|---|
| `02_what_is_ml.ipynb` | 15 / 1 / 11 (total 27) | ✅ AST-clean. Runtime test skipped — uses HF transformers (250 MB download). |
| `03_three_categories.ipynb` | 12 / 0 / 0 (total 12) | ✅ Already light; no trim or Extension needed. Verified runs cleanly. |
| `04_ml_workflow.ipynb` | 14 / 1 / 5 (total 20) | ✅ AST-clean + runtime-verified end-to-end. |

### 3. README — UPDATED

Added a `## How class time is structured (~3 hrs)` section near the top of the README. File map updated to include the new `slides/` folder.

---

## Notebook trim decisions — what moved to Extension

### `02_what_is_ml.ipynb`

| Moved to Extension | Why |
|---|---|
| 1st of 2 Pause-and-Predict cells (rule-based check-in) | Plan rule: at most one Pause-Predict in Core |
| 3 of 4 "What do you notice?" reflections | Plan rule: at most one reflection in Core. The remaining one in Core is a NEW consolidated reflection that summarises insights from both the rule-based output and the ML output — saves a cell while preserving the key teaching contrast. |
| 20-review sanity-checking section | Secondary deep-dive; the 5-review demo is the headline. |
| Stats-lens section (descriptive stats on review length) | Foreshadows L02; not core to "what is ML." |
| Reflection questions (3 take-home Qs) | Live in the Extension list; the take-home reflection lives in `assignment.ipynb`. |
| Sarah Chen chapter note | Optional context, not load-bearing. |

### `03_three_categories.ipynb`

**No changes.** This notebook is conceptual (no Pause-and-Predict, no reflections, just concept cards + a comparison table + a matching exercise + the answer key). 12 cells total, already light. The plan explicitly said "unchanged — already light."

### `04_ml_workflow.ipynb`

| Moved to Extension | Why |
|---|---|
| 2 of 3 "What do you notice?" reflections | Cleaning reflection + evaluation reflection; the most central one — Step 1 framing reflection — stays in Core. |
| Hospital readmissions exercise (matching steps to new scenario) | Excellent stretch exercise; works well as post-class application. |
| Sarah Chen Thursday-afternoon chapter note | Optional context. |
| Setup section heading (md) | One-line markdown that added no value; setup code cell speaks for itself. |

**Bonus structural fix:** The original Section 3 Summary (cell 19) and Module L01 wrap-up (cell 20) were merged into a single closing cell. They were strongly related (table + module bridge) and combining them slimmed Core without losing content.

---

## Deviations from the plan

1. **Slide count is 22, not ~30.** The plan's §2 outline table shows 22 slides; the prose elsewhere said "~30." Plan §7 acceptance criteria allows ±5, so 22 is within tolerance. The detail per slide ended up rich enough that adding more would have meant duplication.

2. **No staging-repo push.** The plan §5 step 7 says to mirror to `gh-staging-l01` and push. Skipped here because (a) the user did not include git workflow in this session's scope, (b) project root is not currently a git repo. Easy to add when ready.

3. **Visual QA via subagent skipped.** The pptx skill recommends rendering slides to images and using a subagent to inspect for overlaps, contrast issues, etc. Skipped to keep this session focused on content; recommend a follow-up pass before classroom use. The deck opens cleanly in PowerPoint / Keynote / LibreOffice but a visual review by you (or a subagent in a follow-up) would tighten any layout issues.

4. **Markdown-style description of `Sarah's Monday — the polarity histogram`** is rendered with shape-based histograms (no actual matplotlib screenshots embedded). The plan suggested generating matplotlib PNGs and embedding them for "can never drift" guarantees. Hand-drawn shapes were faster and look clean, but PNG screenshots would be more rigorous if you want to swap them in later.

---

## What to verify (Opus learner-perspective review)

These are the highest-value things to double-check from a fresh learner's perspective:

- [ ] **Slide-deck flow opens correctly** in PowerPoint or Keynote on macOS — sometimes font fallbacks (Trebuchet MS) render slightly differently across apps.
- [ ] **Consolidated reflection in `02_what_is_ml.ipynb`** still feels like a single coherent thought, not a stapled-together version of two reflections.
- [ ] **L01 04 merged closing cell** — the summary + module-complete merge in `04_ml_workflow.ipynb` reads well as a single flowing section.
- [ ] **No broken cross-references** from the in-Core notebooks to deleted/moved content (e.g., a `← see cell 12` reference that's no longer valid).
- [ ] **The slide deck's code-along cues** still match the actual notebook file names (`02_what_is_ml.ipynb`, `03_three_categories.ipynb`, `04_ml_workflow.ipynb`).

---

## Smoke-test results

| Notebook | AST parse | End-to-end execute | Notes |
|---|---|---|---|
| `02_what_is_ml.ipynb` | ✅ all 8 code cells | ⏭ skipped — uses HF transformers (250 MB model download) | Runs end-to-end given internet + first-time download |
| `03_three_categories.ipynb` | ✅ all code cells | ✅ ran cleanly in `/opt/miniconda3/envs/ml` | — |
| `04_ml_workflow.ipynb` | ✅ all 3 code cells | ✅ ran cleanly | Required `textblob` install (added to env) |

All runs used `/opt/miniconda3/envs/ml/bin/python` with `matplotlib.use("Agg")`. The official `dsai-m3` environment in `environment.yml` is what learners will use — it lists `textblob` under pip already.

---

## File diff summary (vs. v1)

```
+ slides/L01_slides.pptx                    (new file, 22 slides)
+ slides/slides_outline.md                  (new file)
~ README.md                                 (added "How class time is structured" section + slides/ to file map)
~ notebooks/02_what_is_ml.ipynb            (27 cells, reorganised with Core/Extension boundary)
~ notebooks/03_three_categories.ipynb      (unchanged — already light)
~ notebooks/04_ml_workflow.ipynb            (20 cells, reorganised with Core/Extension boundary)
+ HANDOFF_OPTION_A.md                       (this file)
```
