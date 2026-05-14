# Restructure Plan — Option A (Slim Parts + Slide Decks)

**Approved by FF on 2026-05-09** in response to instructor feedback that the L01/L02 in-class notebooks are too dense for a 3-hour class with slides + Q&A + troubleshooting.

**Scope:** L01 (Intro to ML) and L02 (Probability & Statistics). Apply the same pattern to L03+ as those are built.

**Predecessor:** L01 published at github.com/flexfengfeng/dsai-m3-l01-intro-ml; L02 at github.com/flexfengfeng/dsai-m3-l02-prob-stats. This is a v2 restructure of both.

---

## 1. The mismatch we're fixing

The instructor's feedback (paraphrased): *"3-hour class budget is realistic only if you account for ~1.5 hours of slides + ~1.5 hours of coding **including Q&A and env troubleshooting**. Current notebook content overruns that."*

Current design has each Part notebook sized at ~30-60 min with no slide budget. **Option A** rebalances:

| Block | Time | What happens |
|---|---|---|
| **Pre-class** | 75 min (unchanged) | Learners prep alone |
| **Slide-driven lecture** | ~90 min | Instructor leads concept exposition (~30 slides) |
| **Hands-on code-alongs** | ~90 min | 3 slim notebooks @ ~30 min each, including transitions/Q&A |
| **After-class** | self-paced | Notebook Extension sections + assignment |

**Net effect:** same total content reaches the learner; class time is right-sized; depth content is clearly labelled and self-paced.

---

## 2. Slide deck structure (per lesson)

### L01 — `slides/L01_slides.pptx` (~30 slides)

| Section | Slides | Time |
|---|---|---|
| Title + housekeeping | 1 | 2 min |
| M1 → M2 → M3 bridge | 2 | 5 min |
| Sarah's Monday — the 10,000 reviews | 2 | 5 min |
| Rule-based programming and why it fails (with one code example) | 3 | 8 min |
| The ML approach: pre-trained model | 2 | 5 min |
| **→ Code-along cue: `02_what_is_ml.ipynb` (Core)** | 1 | **30 min** |
| Three categories of ML (one slide each) | 3 | 8 min |
| **→ Discussion cue: `03_three_categories.ipynb` (Core)** | 1 | 10 min |
| The 7-step ML workflow (diagram + time-split) | 3 | 8 min |
| Framing — Step 1 is where projects succeed/fail | 1 | 3 min |
| **→ Code-along cue: `04_ml_workflow.ipynb` (Core)** | 1 | 20 min |
| "How sure are we?" — bridge to L02 | 1 | 3 min |
| Assignment preview + Q&A buffer | 1 | 10 min |

### L02 — `slides/L02_slides.pptx` (~32 slides)

| Section | Slides | Time |
|---|---|---|
| Title + Priya's recap | 2 | 4 min |
| Sarah's Monday — the polarity histogram | 2 | 5 min |
| Distributions: the three shapes (normal / right / left) | 3 | 7 min |
| Mean vs median — when to use which | 2 | 5 min |
| Z-scores: standardisation | 2 | 5 min |
| **→ Code-along cue: `02_distributions.ipynb` (Core)** | 1 | **30 min** |
| The sampling problem (84% from 200 reviews) | 2 | 5 min |
| The Central Limit Theorem (Skittles analogy → demo) | 3 | 8 min |
| Confidence intervals + the formula | 2 | 5 min |
| The most common CI mis-reading | 1 | 3 min |
| **→ Code-along cue: `03_confidence_intervals.ipynb` (Core)** | 1 | **30 min** |
| A/B testing — framing Aisha's coupon | 2 | 5 min |
| The p-value + the three mis-readings | 3 | 8 min |
| **→ Code-along cue: `04_ab_testing.ipynb` (Core)** | 1 | 20 min |
| Friday wrap-up + L03 preview + Q&A | 2 | 8 min |

### Slide design conventions (apply to both decks)

- **Format:** PowerPoint (`.pptx`), one master slide template, sans-serif font, generous whitespace
- **Speaker notes inline** — every slide has a 2-4 sentence script in the notes pane
- **Code on slides** — keep snippets <8 lines; full code lives in notebooks
- **Screenshots** — embed key visualisations (a sentiment-distribution plot, a CLT-of-dice plot, an A/B-test bar chart) so learners see results without leaving the slide flow
- **Code-along cue slides** — these are the boundary markers between lecture and hands-on. Format: large notebook filename, "Open this notebook now," 30-min timer suggestion, key learning goal in one sentence
- **Use the `pptx` skill** when generating

### Speaker notes substance

Every slide's notes should answer: *what do you say while this slide is up?* Include:
- The one-line pedagogical goal of the slide
- The key idea in plain English (no jargon without translation)
- Any common learner question to anticipate
- Transition cue to the next slide

This makes the deck usable by a substitute instructor who hasn't internalised Sarah's narrative arc yet.

---

## 3. Notebook trim spec

Apply to all six in-class notebooks (3 per lesson × 2 lessons).

### Hard rules

1. **Add a Core / Extension boundary.** A horizontal rule (`---`) followed by a markdown cell:
   ```
   ## 🟡 Extension — self-study after class
   *Skipping this section will not affect your understanding of later lessons.
    Come back to it when you have time and want to go deeper.*
   ```

2. **Above the line — keep only:**
   - The scenario opener (one markdown cell)
   - The setup cell (imports)
   - **At most one** Pause-and-Predict cell
   - The headline data display (one chart or one summary table)
   - **At most one** "What do you notice?" reflection
   - The closing summary table + "Up next" pointer

3. **Below the line — move:**
   - Secondary plots (e.g., the cell-13 visualisation in `02_distributions` if cell 6 already shows the distribution)
   - The bonus sensitivity tables (e.g., the sample-size table in `03_confidence_intervals` cell 14)
   - Additional Pause-and-Predicts beyond the first
   - Z-score outlier deep-dive cells
   - Anything labelled "Bonus"

4. **Re-test after trimming.** All six notebooks must pass:
   - AST smoke test (every code cell parses)
   - End-to-end execution (all cells in order, no errors)
   - Cross-cell reference check (variables used in Extension cells must exist after Core cells run)

5. **Keep narrative continuity.** The closing "Up next" pointer must still flow into the next notebook. The Sarah-week timeline (Mon/Tue/Wed/Thu/Fri) stays consistent.

### Per-notebook target sizes

| Notebook | Current | Target Core | Target Extension |
|---|---|---|---|
| L01 `02_what_is_ml.ipynb` | ~25 cells, ~30 min | ~15 cells, ~25 min | ~10 cells |
| L01 `03_three_categories.ipynb` | ~10 cells, ~25 min | unchanged (already light) | none needed |
| L01 `04_ml_workflow.ipynb` | ~21 cells, ~30 min | ~14 cells, ~20 min | ~7 cells |
| L02 `02_distributions.ipynb` | 16 cells, ~60 min | ~10 cells, ~30 min | ~6 cells |
| L02 `03_confidence_intervals.ipynb` | 16 cells, ~60 min | ~10 cells, ~30 min | ~6 cells |
| L02 `04_ab_testing.ipynb` | 14 cells, ~60 min | ~10 cells, ~20 min | ~4 cells |

---

## 4. Files Sonnet needs to produce

### L01

```
learner-edition/L01-intro-ml/
├── slides/
│   ├── L01_slides.pptx               ← NEW (~30 slides, speaker notes inline)
│   └── slides_outline.md             ← NEW (the table from §2 above as a quick reference)
├── notebooks/
│   ├── 02_what_is_ml.ipynb           ← TRIM
│   ├── 03_three_categories.ipynb     ← TRIM (light)
│   └── 04_ml_workflow.ipynb          ← TRIM
├── README.md                          ← UPDATE (mention slides; update class structure)
└── HANDOFF_OPTION_A.md                ← NEW (Sonnet writes; lists what was built)
```

### L02

```
learner-edition/L02-prob-stats/
├── slides/
│   ├── L02_slides.pptx               ← NEW (~32 slides, speaker notes inline)
│   └── slides_outline.md             ← NEW
├── notebooks/
│   ├── 02_distributions.ipynb        ← TRIM
│   ├── 03_confidence_intervals.ipynb ← TRIM
│   └── 04_ab_testing.ipynb           ← TRIM
├── README.md                          ← UPDATE
└── HANDOFF_OPTION_A.md                ← NEW
```

### README.md updates (both lessons)

Add a "How class time is structured" subsection somewhere visible:

```
## How class time is structured (~3 hrs)

| Phase | Time | Format |
|---|---|---|
| Slide-driven lecture | ~90 min | Instructor presents core concepts using `slides/L0X_slides.pptx` |
| Hands-on code-alongs | ~90 min | Three notebooks (~30 min each) — Core sections only |
| (Self-study after class) | self-paced | Each notebook has a 🟡 Extension section for going deeper |
```

---

## 5. Suggested execution order (for Sonnet)

1. Trim the L02 notebooks first (they're heavier and the design is fresher in everyone's mind). Run smoke tests after each.
2. Trim the L01 notebooks.
3. Build the L01 slide deck via the `pptx` skill — match the §2 structure, embed two screenshots from notebook outputs.
4. Build the L02 slide deck — same pattern, three screenshots.
5. Write `slides_outline.md` for each lesson (just convert the §2 table to standalone markdown).
6. Update each `README.md` with the new "How class time is structured" section.
7. Mirror to `gh-staging-l01` and `gh-staging-l02`, run the AST + execution smoke tests, commit and push as a single semantic commit per repo:
   - L01 commit message: *"Restructure for slide-driven 3hr class (Option A): slim notebooks + slide deck"*
   - L02 commit message: same pattern
8. Write `HANDOFF_OPTION_A.md` in each lesson folder summarising:
   - What was built
   - Any deviations from this plan
   - What Opus should re-verify in the learner-perspective review

After Sonnet's handoff, Opus reviews from a learner perspective (same pattern as L02 v1 review), and both repos get a polish commit if needed.

---

## 6. Open questions / parking lot

1. **Slide template / branding.** Use a default plain pptx template, or does FF have a SkillsUnion / NorthStar-themed template? **Default if no answer:** plain template with a custom blue accent colour (`#1F4E79`) matching the lesson-redesign skill aesthetic.
2. **Screenshots in slides.** Generate them programmatically (matplotlib → PNG, embed) or use placeholder boxes for FF to fill in? **Default:** generate programmatically using the same data as the notebooks so they can never drift out of sync.
3. **L03+ scope.** Apply this template to L03 from the start? **Default: yes.** Update the L03 plan (when it's drafted) to assume slides + slim notebooks from day one.
4. **Slide source format.** PPTX only, or also a `slides.md` (Marp / Pandoc-compatible) for diff-friendly versioning? **Default: PPTX only for now;** a markdown source can be added later if the team wants version control on slide content.

---

## 7. Acceptance criteria for Sonnet's sprint

When Sonnet finishes, all of the following must be true:

- ✓ All 6 trimmed notebooks AST-parse and execute end-to-end (matplotlib non-interactive backend OK)
- ✓ Each notebook has a clearly visible 🟡 Extension boundary at the right place
- ✓ `slides/L01_slides.pptx` and `slides/L02_slides.pptx` exist and open without errors
- ✓ Slide count is within ±5 of the §2 targets
- ✓ Every slide has speaker notes ≥ 2 sentences
- ✓ READMEs updated with the new class-structure section
- ✓ Both gh-staging repos pushed; `git log` on each shows the new restructure commit
- ✓ `HANDOFF_OPTION_A.md` written for each lesson
