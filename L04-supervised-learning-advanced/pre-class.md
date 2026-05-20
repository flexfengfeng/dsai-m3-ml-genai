# Before Class — L04 — Trees, Ensembles & Tuning

**Estimated time: ~25 minutes.** Complete this before class.

This is the simplest version of "show up prepared": run one notebook, answer a few questions. You'll come to class having seen the idea in action.

| Step | Time | What you do |
|---|---|---|
| **1. Try it** | ~20 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | ~5 min  | Three short questions below |

---

## Step 1 — Try it (~20 min)

Open **`notebooks/01_monday_morning.ipynb`** in VS Code with the `dsai-m3` kernel. Run every cell top to bottom. Read the markdown between cells. Don't skip any cell.

Marcus's Friday request: *"Can you make this model BETTER next week? Try those tree-based models you mentioned."* Same dataset as L03. Sarah trains a single decision tree (overfits), then a random forest (much better), then a tuned gradient-boosted model (a hair better still). The notebook closes with the honest finding that on this dataset all three live within 0.014 F1 of each other.

If this is your first time running a notebook in this repo, see [setup.md](./setup.md) once — you only need to do this for the first lesson.

---

## Step 2 — Quick reflection (~5 min)

Write a sentence or two for each. You can scribble in a notebook, in a journal, or just hold the answer in your head — what matters is that you *tried*.

**Q1. Why does a single decision tree overfit so badly?**

Look at the train F1 vs test F1 in the notebook. What does the gap tell you?

**Q2. Bagging vs boosting in one sentence each.**

Random Forest = bagging. Gradient Boosting = boosting. What's the conceptual difference?

**Q3. Tuning trade-off.**

GridSearchCV with 100 combinations on 5-fold CV runs 500 model fits. When is that worth it, and when isn't it?

---

## Bring to class

- Your answer to Q1.
- One hyperparameter you'd most want to tune for a tree-based model, and why.

---

**Want to go deeper before class?** See **[reference.md](./reference.md) → *Further reading & watching*** at the bottom — videos and recommended readings that used to live in this guide.

**Ran out of time?** Doing just Step 1 (running the notebook) is enough. The class builds on having felt what the lesson teaches; the reflection questions get re-asked live.
