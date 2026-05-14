# L02 Handoff — For Opus Review Pass

**Built by:** Sonnet 4.6 execution sprint  
**Date:** 2026-04-25  
**Plan followed:** `learner-edition/L02-prob-stats/PLAN.md` (status: approved by FF on 2026-04-25, §8 defaults locked)  
**Predecessor:** L01 in `learner-edition/L01-intro-ml/` (used as structural + tonal reference throughout)

---

## What Was Built

All 12 files from PLAN.md §5 exist and are complete:

```
learner-edition/L02-prob-stats/
├── README.md                           ✅ Navigation hub, L01→L02→L03 bridge, 3-phase flow, Core/Optional
├── setup.md                            ✅ L01's setup.md adapted; added scipy/statsmodels note
├── pre-class.md                        ✅ 75-min Kolb 4-stage guide; 3 exercises with sample answers
├── lesson.md                           ✅ Full concept reference; 3 Quick Check blocks; 10 Check questions; L03 bridge
├── reference.md                        ✅ StatQuest/3B1B/Seeing Theory links; 23-term glossary; Cast of Characters
├── environment.yml                     ✅ L01 yml + scipy + statsmodels
└── notebooks/
    ├── 01_monday_morning.ipynb         ✅ Pre-class hook; polarity histogram; mean vs median; Priya's pushback
    ├── 02_distributions.ipynb         ✅ Tuesday; normal/skewed; Z-scores; Pause-and-Predict; summary table
    ├── 03_confidence_intervals.ipynb  ✅ Wednesday; bootstrap demo; CLT dice; CI formula; mis-reading flag
    ├── 04_ab_testing.ipynb            ✅ Thursday; Marcus intro; z-test; 3 mis-readings; L03 pivot cell
    ├── assignment.ipynb               ✅ Tom/Lakeside (practice) + hospital survey (exercises); Tier 1/2/3; solutions
    └── optional_extensions.ipynb     ✅ Bayes screening test; t-test derivation; bootstrapping; CLT proof sketch
```

### Quality checks run
- **AST smoke test:** 6 notebooks, 45 code cells — all pass cleanly.
- **Cross-file grep audit:** instructor-edition paths: 0 hits in content files; legacy file names (practice.ipynb, quiz.md, studies.md): 0 hits in content files; M1/M2 leakage: 0 hits in content files. All hits were in PLAN.md only (expected).
- **Tier 2 safety check:** All `None`-initialised placeholders parse cleanly; no `___ inside f-string` pattern found.
- **Cross-notebook pointer check:** All `Up next →` references in notebooks resolve to existing files.

---

## §8 Locked Decisions — Execution Notes

1. **Marcus intro in Thursday A/B scene:** ✅ Done. Marcus gets a one-liner introduction ("NorthStar's CFO") when he first appears in `04_ab_testing.ipynb` scenario cell.
2. **Hand-labelling sample = 200 reviews → 84% accuracy:** ✅ Done. In `03_confidence_intervals.ipynb`, the simulation uses `SAMPLE_SIZE = 200` and `TRUE_ACCURACY = 0.83` (so Sarah's sample produces ~84%). The value is not shrunk.
3. **A/B test metric = first-30-days complaint rate:** ✅ Done. In `04_ab_testing.ipynb`, the metric is explicitly "30-day complaint rate" for NEGATIVE-tagged customers. Binary outcome, two arms.
4. **Assignment domain = hospital satisfaction survey:** ✅ Done. `assignment.ipynb` has an explicit callback: "Sarah's been on a hospital project before — in the L01 assignment." St. Cecilia's Hospital is the setting.
5. **Bayes in `optional_extensions.ipynb`:** ✅ Done. One worked example (breast-cancer-style screening test), one Quick Check, and no broader Bayesian inference scope.

---

## Deviations From Plan

### Minor deviations (intentional, document for Opus review)

1. **Two-proportion z-test instead of t-test in `04_ab_testing.ipynb`:** The plan says "Run a synthetic A/B test in code (binomial outcomes, two arms)" and "Compute and interpret a p-value with `scipy.stats`." I used `scipy.stats.proportions_ztest` (the correct test for binomial/proportion outcomes) rather than a t-test. A t-test is for comparing means; `proportions_ztest` is for comparing rates. This is statistically correct. I also show a contingency table via `chi2_contingency` for completeness. **Recommend Opus confirm the learner audience is fine with two tests being shown.**

2. **Case study notebook not built:** PLAN.md §5 does not include a `case_study.ipynb` (unlike the generic skill template). The plan's file map has 6 notebooks matching the L01 pattern exactly — no case study. Confirmed not required.

3. **`lesson.md` has 10 "Check your understanding" questions at the bottom** (beyond the 3 Quick Checks per Part). This mirrors L01's lesson.md pattern which also has section Quick Checks + a comprehensive 10-question end-of-document check. Sonnet added this because L01 has it and the plan says "mirror L01."

4. **Instructor-edition mirror not created:** PLAN.md §9 says "Notebooks are also mirrored to `instructor-edition/L02-probability-statistics/`" with a note that this is an "Open question." Since §8 says open questions should be treated as defaults (not locked decisions), and the plan's §10 execution order does not include this step, I left the instructor-edition mirror out. **Flag for Opus and FF to decide before push.**

5. **`pre-class.md` Stage 3 has 4 sub-parts (A/B/C/D) instead of 3:** The plan's Phase 1 table shows 30 min for "Learn" with videos + lesson.md. I split this into: Part A (StatQuest Normal Distribution, 10 min), Part B (StatQuest CI, 10 min), Part C (lesson.md preview, 5 min), Part D (peek at Part notebooks, 5 min). Total is still 30 min; the split makes the guidance more actionable.

### Things I was uncertain about

1. **Marcus is "CFO" in this handoff but "CTO" in the L01 `reference.md` Cast of Characters.** L01's reference.md says "Marcus Wong — CTO." The L02 PLAN.md §8 decision says "Marcus (CFO)." I followed the PLAN.md §8 locked decision (CFO) throughout L02. **Opus should reconcile the character role: is Marcus CTO or CFO across the whole course? If CTO in L01 was intentional, L02 should be corrected to CTO. If the role changed, both lesson files need updating.** I have not touched L01.

2. **assignment.ipynb hospital scenario score range:** Satisfaction scores are 1–10 (common survey scale). The binary "highly satisfied" threshold is score ≥ 8. This is a reasonable choice but not specified in the plan. Opus should check this is appropriate given the L01 hospital assignment context.

3. **StatQuest video links:** I verified the video titles and channels used (StatQuest with Josh Starmer, 3Blue1Brown) but did not verify that the specific YouTube URLs are still live — they were sourced from memory and standard StatQuest/3B1B libraries. **Opus should verify all video links in `pre-class.md` and `reference.md` resolve correctly before publishing.**

4. **`environment.yml` does not add `statsmodels`** — I added it to the YAML but did not use `statsmodels` in any notebook (all tests are via `scipy.stats`). The plan says "verify scipy/statsmodels are present." I added both but only scipy is used. Statsmodels is available for learners who explore further.

5. **`01_monday_morning.ipynb` uses synthetic data** that approximates L01's polarity results (beta-distribution mix to produce a realistic right-ish skewed polarity distribution). I could not access Sarah's actual L01 notebook outputs. The synthetic data is seeded (np.random.seed(42)) so it's reproducible and produces ~60% "positive" as specified. If L01 produces a saved `.csv` of polarity scores, the hook notebook should be updated to load that file instead.

---

## Open Items for Opus

In rough priority order:

1. **Marcus role (CFO vs CTO):** Reconcile across L01 `reference.md` and all L02 files before push.
2. **Instructor-edition mirror:** Decide whether L02 notebooks should be mirrored to `instructor-edition/` and, if so, whether to use `Part_1_…` naming or keep the narrative `02_distributions…` naming.
3. **Video link verification:** Spot-check all YouTube links in `pre-class.md` and `reference.md` resolve.
4. **Learner perspective review:** Run through the full Kolb cycle as a learner: pre-class → 01 hook → Parts 1–3 → assignment. Check all "Up next" pointers, all Pause-and-Predict positions, all sample answers.
5. **Two-proportion z-test pedagogy:** Confirm showing both `chi2_contingency` and `proportions_ztest` in `04_ab_testing.ipynb` is not confusing for adult learners with no statistics background. If confusing, remove `chi2_contingency` and keep only `proportions_ztest`.

---

## What Sonnet Did Not Do

- Push to GitHub (per plan instructions: "Do NOT push to GitHub — leave that for the Opus review pass")
- Create assets/ folder or infographic images (plan does not specify L02 infographics; L01 has the folder but it may be empty)
- Write a CLAUDE.md or update any course-level index files
- Touch any L01 files

---

*This handoff is ready for Opus 4.7 learner-perspective review.*
