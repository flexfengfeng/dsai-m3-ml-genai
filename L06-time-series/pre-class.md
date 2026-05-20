# Before Class — L06 — Time Series Forecasting

**Estimated time: ~25 minutes.** Complete this before class.

This is the simplest version of "show up prepared": run one notebook, answer a few questions. You'll come to class having seen the idea in action.

| Step | Time | What you do |
|---|---|---|
| **1. Try it** | ~20 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | ~5 min  | Three short questions below |

---

## Step 1 — Try it (~20 min)

Open **`notebooks/01_monday_morning.ipynb`** in VS Code with the `dsai-m3` kernel. Run every cell top to bottom. Read the markdown between cells. Don't skip any cell.

Marcus needs a Q4 holiday revenue forecast for the board. The notebook hands Sarah two years of daily revenue. She tries a Naive baseline, learns about Seasonal Naive + ETS, then builds an ML forecaster with lag features. The honest finding: on a single holiday window, fancier methods can tie the naive baseline — *single-window evaluation lies. Always cross-validate.*

If this is your first time running a notebook in this repo, see [setup.md](./setup.md) once — you only need to do this for the first lesson.

---

## Step 2 — Quick reflection (~5 min)

Write a sentence or two for each. You can scribble in a notebook, in a journal, or just hold the answer in your head — what matters is that you *tried*.

**Q1. Why is random k-fold dangerous on time series?**

What goes wrong? (One sentence.)

**Q2. Naive sometimes beats your fancy model on a single window. Why?**

What should you do about it?

**Q3. Pick the seasonal period.**

Daily ice-cream sales over 3 years — what `period=` would you use for STL? Why?

---

## Bring to class

- Your answer to Q2.
- One time-series problem from your own domain (could be anything: web traffic, energy demand, headcount).

---

**Want to go deeper before class?** See **[reference.md](./reference.md) → *Further reading & watching*** at the bottom — videos and recommended readings that used to live in this guide.

**Ran out of time?** Doing just Step 1 (running the notebook) is enough. The class builds on having felt what the lesson teaches; the reflection questions get re-asked live.
