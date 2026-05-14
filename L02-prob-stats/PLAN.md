# L02 — Probability & Statistics for ML · Redesign Plan

**Status:** Approved by FF on 2026-04-25 (defaults accepted on all open questions). This brief is now the spec a Sonnet 4.6 execution session should follow.
**Predecessor:** L01 redesign in `learner-edition/L01-intro-ml/` (live at github.com/flexfengfeng/dsai-m3-l01-intro-ml).
**Methodology:** Apply the `lesson-redesign` skill end-to-end, mirroring the L01 file map and Kolb structure.

---

## 1. Where L02 sits

| Lesson | Role | What learner carries forward |
|---|---|---|
| **L01 — Intro to ML** | The "what" and "why" of ML, plus a felt run of a sentiment model on Sarah's reviews. Closes on Priya asking *"How sure are we?"* | A working sentiment model and an unanswered question about its trustworthiness. |
| **L02 — Probability & Statistics for ML** *(this lesson)* | The formal vocabulary for answering "how sure are we?" and "did this intervention actually work?" | Distributions, confidence intervals, p-values — the lens every later model is judged through. |
| **L03 — Supervised Learning** | First model trained from labelled data; uses the L02 vocabulary to evaluate it. | Cross-validation and confusion matrices read in statistical terms. |

**Narrative throughline:** Sarah produced a number in L01. In L02 she has to *defend* the number to Priya and learn to *test* whether interventions on top of it actually move outcomes.

---

## 2. Learning objectives (carried from existing roadmap)

By the end of L02 the learner will be able to:

1. **Read** a distribution shape (normal, skewed, bimodal) and explain why it changes how a model is built and how a result is reported.
2. **Compute and interpret** a confidence interval around a sample statistic and articulate what it does and doesn't say.
3. **Design and read** a basic A/B test, including the p-value, the assumptions behind it, and the most common mis-readings.
4. **Choose between** descriptive ("what happened") and inferential ("what's likely true beyond this sample") statistics for a given business question.
5. **Recognise** the central limit theorem (CLT) at work and use it to explain why sample means are well-behaved even when the underlying data isn't.

(These match the M3 roadmap Core list for L02. No change requested — flagging here so Sonnet doesn't drift.)

---

## 3. Sarah's week (the narrative)

**Monday.** Priya re-opens the Friday meeting from L01: *"Your model says 60% of reviews are positive. Is the real rate 60%, or could it be 50% and we got lucky on this batch?"*

**Tuesday — Distributions.** Sarah pulls the sentiment polarity scores for all 10,000 reviews. They're not normally distributed. She has to explain to Priya what "skewed" means and why averaging skewed data is misleading.

**Wednesday — Confidence intervals.** Sarah hand-labels 200 reviews to ground-truth her model's accuracy. She gets 84%. Priya asks *"so the real accuracy is exactly 84%?"* — and Sarah learns to put a confidence interval around it: *"between 78% and 89%, with 95% confidence."*

**Thursday — A/B testing.** Aisha proposes sending an automated apology coupon to every NEGATIVE-flagged customer. Marcus (CFO) asks *"will this actually reduce churn or just cost us money?"* Sarah designs an A/B test: half the negative-tagged customers get the coupon, half don't. She runs it for two weeks and reports the result with a p-value.

**Friday.** Sarah presents three numbers — a sentiment rate, an accuracy interval, and an A/B test result — each one defended with the right statistical lens. Priya nods. Marcus nods. The model survives leadership review.

**The closing pivot for L03:** Marcus says *"OK, the sentiment model holds up. But you used a pre-trained one. Can you build us a model that predicts churn from our own data?"* — that's the L03 hook.

---

## 4. Three-Phase Kolb structure

Same three-phase Kolb arc as L01, same timings, same file rhythm.

### Phase 1 — Pre-class self-study (~75 min)

Estimated time: 75 min. Sarah's *Monday morning* — the felt experience of "I have a number but I can't defend it yet."

| Stage | Time | What the learner does |
|---|---|---|
| **1. Try it** | 15 min | Run `01_monday_morning.ipynb` — Sarah's polarity histogram, a single mean, and Priya's pushback |
| **2. Reflect** | 10 min | Reflection prompts (where would 60% mislead? what would *defend* the number?) |
| **3. Learn** | 30 min | StatQuest videos (CLT, p-values) + preview `lesson.md` |
| **4. Practise** | 20 min | 3 mini-exercises with sample answers |

### Phase 2 — In-class (~3 hrs)

Three Part notebooks, each opens with a continuation of Sarah's week, follows a Pause-and-Predict / What-do-you-notice / Summary-table rhythm, and ends on the next narrative question.

| # | Notebook | Topic | Sarah's day |
|---|---|---|---|
| 02 | `02_distributions.ipynb` | Distribution shapes · normal vs skew · Z-scores · why shape matters for modelling | Tuesday — explain "skewed" to Priya |
| 03 | `03_confidence_intervals.ipynb` | Sampling · CLT · confidence intervals · margin of error | Wednesday — "between 78% and 89%" |
| 04 | `04_ab_testing.ipynb` | A/B testing · p-values · the most common mis-readings | Thursday — the apology coupon |

### Phase 3 — After-class (self-paced)

`assignment.ipynb` — Sarah is back at Lakeside Bank for one day. **Tom Bradley** wants to know whether a new mobile-app onboarding flow is reducing complaint rates. Same three lenses (distribution, CI, A/B), different data.

`optional_extensions.ipynb` — Bayes' theorem math, t-test formula derivation, bootstrapping theory, full CLT proof. Curious-learner only.

---

## 5. File map (target)

Mirrors the L01 layout exactly, so the gh-staging publish + regenerate scripts can be reused with minimal change.

```
learner-edition/L02-prob-stats/
├── README.md                       ← Navigation hub, M1→M2→M3→L02 bridge, 3-phase flow
├── setup.md                        ← Reuse L01's almost verbatim — same env
├── pre-class.md                    ← Phase 1 guide (75 min)
├── lesson.md                       ← Concept reference: each topic gets plain-English / analogy / ML-relevance / Quick Check
├── reference.md                    ← Further reading + glossary (~20 terms)
├── environment.yml                 ← Reuse L01's; verify scipy/statsmodels are present
└── notebooks/
    ├── 01_monday_morning.ipynb     ← Pre-class hook (~15 min runtime)
    ├── 02_distributions.ipynb      ← Part 1 — Tuesday
    ├── 03_confidence_intervals.ipynb ← Part 2 — Wednesday
    ├── 04_ab_testing.ipynb         ← Part 3 — Thursday
    ├── assignment.ipynb            ← After-class — Tom Bradley at Lakeside Bank, complaint-flow A/B test
    └── optional_extensions.ipynb   ← Bayes math · t-test derivation · bootstrap · CLT proof
```

---

## 6. Core / Optional split (from roadmap, locked)

🟢 **Core (taught in class):**
- Distributions formalised — normal, skewed, Z-scores
- Confidence intervals + CLT
- A/B testing with p-values

🟡 **Optional (self-study, not assessed):**
- Bayes' theorem math
- t-test formula derivation
- Bootstrapping theory
- Full CLT proof

The README and the Phase-2 notebooks should each carry the same Core/Optional callout L01 uses, with a link to `optional_extensions.ipynb`.

---

## 7. Notebook detail

### `01_monday_morning.ipynb` (pre-class hook, ~15 min)

- Sarah loads her L01 sentiment results: `60% positive, 40% negative`.
- Histograms the polarity distribution — clearly skewed.
- Computes one mean. Compares to median. Notices they disagree.
- Priya's voice in the closing markdown: *"60%? Could it have been 50% if we had a different week of reviews?"*
- Ends pointing the learner at `pre-class.md` for the four-step rhythm.

### `02_distributions.ipynb` (Part 1, Tuesday)

- **Concept block:** what a distribution is in plain English. Real-world analogy: heights of adults vs household incomes.
- **Hands-on:** plot polarity, plot review-length, plot a synthetic income vector. Identify shapes.
- **Concept block:** Z-scores — *"how many standard deviations from average?"* Useful for outlier detection and feature scaling.
- **Pause-and-Predict:** before plotting review length, ask the learner to guess the shape.
- **Closing summary table** + pointer to Part 2.

### `03_confidence_intervals.ipynb` (Part 2, Wednesday)

- **Sarah's hand-label exercise** — she labels 200 reviews, gets 84% accuracy.
- **Sampling intuition:** repeat the 200-review labelling 1,000 times in code by resampling from the labelled set; show the distribution of accuracies.
- **The CLT in one paragraph + one demo cell** (sums of dice / non-normal source → normal sample-mean distribution). No formula proof.
- **Confidence intervals:** the bracket around 84%. Plain-English interpretation, plus the most common mis-reading (*"95% chance the true value is in this range"* — wrong, but worth flagging).
- **Pause-and-Predict** before computing the bracket.
- **Closing:** *"now Sarah can write '84% (95% CI 78–89%)' in her deck. Tomorrow Aisha pitches the apology coupon."*

### `04_ab_testing.ipynb` (Part 3, Thursday)

- Aisha's apology coupon — frame the test (control, treatment, metric, sample size).
- Run a synthetic A/B test in code (binomial outcomes, two arms).
- Compute and interpret a p-value with `scipy.stats`.
- The three most common mis-readings of a p-value — explicit, with the corrected reading next to each.
- **Pause-and-Predict** on whether the simulated effect will be "significant".
- Closing summary table + pointer to the assignment.
- **Final cell — Marcus's pivot to L03:** *"Now build me a model from our own data."*

### `assignment.ipynb` (after-class)

- **Tom Bradley returns** — Lakeside Bank rolled out a new onboarding flow. Tom wants to know whether the new flow reduces "first-30-days complaint rate."
- Three tiers (Guided / Partial / Open) mirroring the L01 assignment template.
- Three independent exercises in a fourth domain (Sonnet should pick a domain consistent with the Cast of Characters). Suggested: a hospital satisfaction survey scenario, since L01's exercise 7 already used hospital readmission.
- Sample solutions at the bottom.

### `optional_extensions.ipynb`

- **Bayes' theorem with the breast-cancer-screening example** (canonical, well-explained in StatQuest).
- **t-test formula derivation** — for learners who want to see where the test statistic comes from.
- **Bootstrapping** — code-only intuition, no asymptotic theory.
- **CLT proof sketch** — pointer to 3Blue1Brown's CLT video plus a short text walk-through.

---

## 8. Decisions (locked by FF on 2026-04-25)

These were open questions in draft v1; FF approved the defaults, so Sonnet should treat them as fixed and not re-litigate.

1. **Marcus (CFO) is voiced in L02's Thursday A/B-test scene.** He's the one asking *"will this actually reduce churn or just cost us money?"* This is his first speaking line in the course; brief intro one-liner when he first appears.
2. **Hand-labelling sample size = 200 reviews → 84% accuracy** for the CI demo on Wednesday. Don't shrink it.
3. **A/B-test outcome metric = first-30-days complaint rate** after sending an apology coupon to NEGATIVE-flagged customers. Binary outcome, two arms (coupon vs no coupon), simulated in code.
4. **Assignment domain = hospital satisfaction survey.** Callback to L01's "apply the workflow" hospital example is intentional. Sonnet should write the assignment so the connection is explicit (*"Sarah's been on a hospital project before — same lens, different question"*).
5. **Bayes is in `optional_extensions.ipynb`.** Use the canonical screening-test example. Keep it short — one worked example, one Quick Check; don't try to teach Bayesian inference more broadly.

---

## 9. Acceptance criteria for Sonnet's execution sprint

When Sonnet finishes, the following must be true:

- All eight files in §5 exist and follow the L01 patterns (file-templates.md / notebook-patterns.md from the lesson-redesign skill).
- Every code cell AST-parses cleanly (re-use the smoke test from L01).
- The L01 closing pointer in `04_ml_workflow.ipynb` (*"how sure are we?"*) is matched by the L02 opening pointer.
- Every reflection / Pause-and-Predict has a sample answer.
- Every Tier 2 in the assignment is syntactically valid Python before the learner fills any blanks.
- README's Core/Optional callout matches §6 exactly; lesson.md mirrors the same split.
- No `instructor-edition` paths or other M1/M2 leakage in any file.
- Notebooks are also mirrored to `instructor-edition/L02-probability-statistics/` with file rename: `01_monday_morning.ipynb` → keep as is, but follow the L01 instructor naming if the user prefers `Part_1_…` style. (Open question — see §8.)

After Sonnet hands back, Opus 4.7 picks it up for the **learner-perspective review pass** (mirroring what we just did for L01) before any push to GitHub.

---

## 10. Suggested order of execution (for Sonnet)

1. Read L01's `learner-edition/L01-intro-ml/` end-to-end as the structural reference.
2. Read `lesson-redesign/references/file-templates.md` and `lesson-redesign/references/notebook-patterns.md`.
3. Mine the legacy `3.1-probability-statistics/` for re-usable text (concepts, exercises, glossary entries).
4. Write `lesson.md` first — the concept reference informs everything else.
5. Write `pre-class.md` and `01_monday_morning.ipynb` together — they share the same hook.
6. Write 02 / 03 / 04 in order; each notebook should reference the previous in its opening.
7. Write `assignment.ipynb` last; it has to depend on all three Part notebooks being settled.
8. Run AST smoke test + the cross-file grep audit (`grep -RIn "hook\.ipynb\|instructor-edition" learner-edition/L02-prob-stats`) before stopping.
9. Write a short `HANDOFF.md` in `learner-edition/L02-prob-stats/` with: what was built, what's unsure, decisions made on the §8 questions if no answer was provided, and any deviations from this plan.

That handoff doc is what Opus 4.7 picks up for the review pass.
