# Before Class — L03 — Supervised Learning

**Estimated time: ~25 minutes.** Complete this before class.

This is the simplest version of "show up prepared": run one notebook, answer a few questions. You'll come to class having seen the idea in action.

| Step | Time | What you do |
|---|---|---|
| **1. Try it** | ~20 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | ~5 min  | Three short questions below |

---

## Step 1 — Try it (~20 min)

Open **`notebooks/01_monday_morning.ipynb`** in VS Code with the `dsai-m3` kernel. Run every cell top to bottom. Read the markdown between cells. Don't skip any cell.

Sarah's job this week: predict which NorthStar customers will churn next quarter. The notebook hands her a 10,000-row customer dataset with 11 features and a `churned` label. She'll train a logistic regression, see "88% accuracy" — and then discover that this catches only 7 of 239 actual churners. *Accuracy is a lie when classes are imbalanced.*

If this is your first time running a notebook in this repo, see [setup.md](./setup.md) once — you only need to do this for the first lesson.

---

## Step 2 — Quick reflection (~5 min)

Write a sentence or two for each. You can scribble in a notebook, in a journal, or just hold the answer in your head — what matters is that you *tried*.

**Q1. 88% accuracy, 7 of 239 churners caught.**

Why is the headline accuracy misleading here? What metric would you report to Marcus instead?

**Q2. Threshold choice as a business decision.**

Sarah has a retention team that can call ~200 customers a week. How should that capacity number affect the model's threshold? (One sentence.)

**Q3. What's a 'pipeline' and why bother?**

Why is wrapping preprocessing + model in one sklearn `Pipeline` better than running the steps separately?

---

## Bring to class

- Your answer to Q1 in one sentence.
- One classification problem from your own domain where the positive class is rare (< 20%).

---

**Want to go deeper before class?** See **[reference.md](./reference.md) → *Further reading & watching*** at the bottom — videos and recommended readings that used to live in this guide.

**Ran out of time?** Doing just Step 1 (running the notebook) is enough. The class builds on having felt what the lesson teaches; the reflection questions get re-asked live.
