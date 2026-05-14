# Before Class — L01 Introduction to Machine Learning

> *Sarah Chen · Customer Experience Analyst · NorthStar Retail · January 2023.*

**Estimated time: 75 minutes.** Complete this before class.

This guide walks you through four short steps. You will *try* an ML model first, *reflect* on what surprised you, *learn* the underlying ideas, and *practise* applying them. You will come to class with a felt sense of what ML can do — and with a question you want to answer.

| Step | Time | What you do |
|---|---|---|
| **1. Try it** | 15 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | 10 min | Reflection prompts below |
| **3. Learn** | 30 min | Watch videos + preview `lesson.md` |
| **4. Practise** | 20 min | 3 mini-exercises with sample answers |

---

## Step 1 — Try it (15 min)

**What to do:** Open **`notebooks/01_monday_morning.ipynb`** in your Python environment. Run every cell top to bottom. Read the markdown between cells. Do not skip any cell.

The notebook drops you into Sarah Chen's first Monday at NorthStar Retail. Aisha Patel from Customer Service has handed Sarah a USB drive with 10,000 reviews. Priya, Sarah's manager, wants answers by Friday.

You will:
- Try to classify 5 reviews using **hand-written rules** (the traditional approach) and watch the rules fail
- Run a **pre-trained ML model** on the same reviews and watch it succeed in seconds
- End on Priya's doubt: *"But are the positive ones actually positive? How do we know?"*

Do this first, before reading anything below.

> **If you have never run a Jupyter notebook before:** see [setup.md](./setup.md). Estimated time: 10–15 extra minutes, one time only.

**Stuck? Troubleshooting:**
- If the TextBlob import fails, run `pip install textblob` in a terminal and restart the kernel.
- If a cell hangs, click the ■ stop button and re-run. None of the cells in `01_monday_morning.ipynb` should take longer than a few seconds.

---

## Step 2 — Reflect (10 min)

Now that you've seen the model work, slow down. Answer these prompts in a notebook, a journal, or just in your head.

**Q1 — The magic moment.**
What happened in the cell where the ML model ran? How long did it take? What would the same task have cost Sarah if she had done it manually?

> *There is no single right answer. Notice what you noticed.*

**Q2 — Where did the rule-based approach break down?**
Look back at the rule-based classifier from earlier in the notebook. Name one review it got wrong, and explain *why* rules struggle with language.

**Q3 — What surprised you?**
Pick one surprise. It can be technical ("I didn't know a model could be one line of code") or business-related ("I didn't realise 10,000 reviews was a week of work"). Write one sentence.

**Q4 — What would you ask the model?**
If you could ask the model one question about *how* it made its decisions, what would it be? Bring this question to class. We will try to answer it.

**Q5 — Priya's doubt.**
The notebook ends with Priya asking *"Are the positive ones actually positive? How do we know?"* Why is this question more important than it sounds?

> **Sample angle (check after you try):** The notebook showed roughly 60% positive. But Sarah already caught *one* sarcastic review the model got wrong. If the model makes the same mistake on, say, a few hundred of the 10,000 reviews, the real positive rate might be closer to 45%. Priya's retention strategy looks very different at 60% happy customers vs 45%. Without a way to *verify*, Sarah cannot defend the number — she can only report it. Lesson 2 is about how to close that gap.

---

## Step 3 — Learn (30 min)

Now read about what actually happened.

### Part A — What Machine Learning is (10 min)

**Watch:** [*Machine Learning explained in 100 seconds*](https://www.youtube.com/watch?v=PeMlggyqz0Y) (Fireship, 2 min)
**Then:** [*What is Machine Learning?*](https://www.youtube.com/watch?v=a0_lo_GDcFw) (CrashCourse AI, ~10 min — watch at 1.25× if short on time)

If those links are unavailable, search YouTube for "StatQuest Machine Learning Fundamentals" and watch the first introduction video.

**While you watch, hold one question in mind:** *What was different about how the computer learned, compared to a program someone wrote by hand?*

### Part B — Preview the concept reference (10 min)

Open [`lesson.md`](./lesson.md). Read the following sections only:
- "Why Machine Learning matters" (top of the file)
- "Part 1: What is Machine Learning?" through the end of "Key vocabulary"
- "Part 2: Comparison at a glance" (the table — just the table)
- "Part 3: The 7 steps" (numbered list only)

**Do not try to read the whole document now.** The Part notebooks will walk you through it in class.

### Part C — Peek at the Part notebooks (10 min)

Open each Part notebook and read just the first markdown cell — the business scenario — then close it.

| Notebook | One-sentence preview |
|---|---|
| `notebooks/02_what_is_ml.ipynb` | Sarah runs the pre-trained model herself and tests its limits. |
| `notebooks/03_three_categories.ipynb` | Sarah meets the three categories of ML and decides which one she's using. |
| `notebooks/04_ml_workflow.ipynb` | Sarah's model goes to the boardroom — can she defend it? |

---

## Step 4 — Practise (20 min)

Three short exercises. Attempt each one *before* looking at the sample answer. The act of trying before looking is what makes the learning stick.

### Exercise 1 — Rule-based vs ML — which one? (~7 min)

For each of the four tasks below, decide whether a traditional rule-based program (clear `if/else` rules) or a Machine Learning model is the better approach. Write one sentence for each saying why.

1. Calculate an employee's tax deduction from their salary
2. Decide whether a photo uploaded to a social app contains a cat or a dog
3. Convert a temperature from Fahrenheit to Celsius
4. Predict which NorthStar customer will cancel their loyalty membership next month

> **Sample answer:**
> 1. **Rule-based.** Tax is defined by exact formulas in the law. An ML model would be overkill and less accurate than the formula.
> 2. **ML.** There is no rule that defines a cat pixel-by-pixel. Patterns are too varied to enumerate, but plenty of labelled photos exist.
> 3. **Rule-based.** It's a single arithmetic formula: `(F - 32) * 5/9`. No data needed.
> 4. **ML.** The signals of "about to cancel" are subtle patterns in behaviour — not one obvious rule. NorthStar's historical data about who did and didn't cancel is the right fuel. (This is exactly L03's chapter.)

### Exercise 2 — Match the problem to the ML category (~7 min)

Match each business problem to the ML category that fits.

| Business problem | Category |
|---|---|
| 1. Detect fraudulent transactions (you have labelled historical cases) | a. Supervised learning |
| 2. Segment NorthStar's 200k customers into interest groups (no labels) | b. Unsupervised learning |
| 3. Train a game AI that gets a reward when it wins | c. Reinforcement learning |
| 4. Predict next quarter's coat sales | |
| 5. Find unusual patterns in server logs (possible security breach) | |

> **Sample answer:**
> 1 → **a** (labelled fraud data)
> 2 → **b** (no labels, discover groups — this is L05's chapter at NorthStar)
> 3 → **c** (reward signal)
> 4 → **a** (historical sales = labels — this is L06's chapter on time-series forecasting)
> 5 → **b** (anomaly detection without labels)

### Exercise 3 — Order the ML workflow (~6 min)

Put these steps in the right order:

- Train the model
- Define the business problem and what success looks like
- Deploy the model to production
- Clean and explore the data
- Collect the data
- Evaluate the model on held-out data
- Monitor the model over time

> **Sample answer:**
> 1. Define the business problem and success
> 2. Collect the data
> 3. Clean and explore the data
> 4. Train the model
> 5. Evaluate on held-out data
> 6. Deploy
> 7. Monitor
>
> **Bonus reflection:** Out of these 7 steps, which one do you think is the most common failure point in real ML projects? (Many practitioners say Step 1 — defining the problem — or Step 7 — monitoring. Come to class with your answer. We'll debate it.)

---

## Bring to the Session

Come ready with:

1. **Your question from Step 2, Q4** — what you would ask the model.
2. **Your answer to the bonus reflection** in Exercise 3 — which step fails most often, and why.
3. **One real-world example from your own work or life** where ML could help (or has helped). Be ready to name which of the three categories it fits into and what "success" would look like.
4. **One concept that didn't fully click.** The session will address it.

You do not need to have read all of `lesson.md` — the instructor will walk you through it. You just need to have run `01_monday_morning.ipynb` and attempted the exercises.

---

## If You Ran Out of Time

Did the full 75 minutes but haven't finished everything? Prioritize in this order:

1. **Run `01_monday_morning.ipynb`** — non-negotiable. The whole class builds on your having felt what the model can do.
2. Attempt Exercise 1 (rule-based vs ML) and Exercise 2 (match the category).
3. Skip the video if you must — the instructor will cover it.

You can come back to the rest after class. The same try → reflect → learn → practise rhythm repeats in class (the Part notebooks) and once more after class (the assignment), so no single miss is fatal.
