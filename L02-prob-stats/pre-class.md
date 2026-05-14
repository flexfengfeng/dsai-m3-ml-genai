# Before Class — L02 Probability & Statistics for Machine Learning

> *Sarah Chen · Customer Experience Analyst · NorthStar Retail · January 2023.*
> It is Monday morning. On Friday, Priya asked the question Sarah couldn't answer: *"Your model says 60% of reviews are positive — but how do we know we can trust that number?"*

**Estimated time: 75 minutes.** Complete this before class.

This guide walks you through four short steps. You will *try* the statistical problem first (in a notebook), *reflect* on what made the number hard to defend, *learn* the underlying ideas, and *practise* applying them in short exercises. You will come to class understanding why "60% positive" is a start — not an answer.

| Step | Time | What you do |
|---|---|---|
| **1. Try it** | 15 min | Open and run `notebooks/01_monday_morning.ipynb` |
| **2. Reflect** | 10 min | Reflection prompts below |
| **3. Learn** | 30 min | Watch videos + preview `lesson.md` |
| **4. Practise** | 20 min | 3 mini-exercises with sample answers |

---

## Step 1 — Try it (15 min)

**What to do:** Open **`notebooks/01_monday_morning.ipynb`** in your Python environment. Run every cell top to bottom. Read the markdown between cells. Do not skip any cell.

The notebook drops you into Sarah's Monday morning at NorthStar Retail. She has a number from last week — "60% positive" — and a question she can't yet answer: *"Could that be 50% if we'd analysed a different batch of reviews?"*

You will:
- Load Sarah's L01 sentiment results and plot the polarity score distribution
- Compute the mean polarity — then notice how it compares to the median
- End on Priya's exact question: *"60% positive. Is the real rate 60%, or could it be 50% and we got lucky on this batch?"*

Do this first, before reading anything below.

> **If you have never run a Jupyter notebook before:** see [setup.md](./setup.md). Estimated time: 10–15 extra minutes, one time only.

**Stuck? Troubleshooting:**
- If the scipy import fails, run `pip install scipy` in a terminal and restart the kernel.
- If a cell hangs, click the ■ stop button and re-run. None of the cells in `01_monday_morning.ipynb` should take longer than a few seconds.

---

## Step 2 — Reflect (10 min)

Now that you've seen the data, slow down. Answer these prompts in a notebook, a journal, or just in your head.

**Q1 — Mean vs median.**
In the notebook, the mean polarity and the median polarity were different. Which one would you use to report "typical" sentiment to Priya, and why?

> *There is no single right answer. Think about what each number actually represents.*

**Q2 — The shape question.**
Look at the histogram of polarity scores. Is it symmetric (bell-shaped) or does it lean in one direction? What does that lean tell you about NorthStar's review corpus?

**Q3 — Where "60% positive" comes from.**
Sarah got "60% positive" by counting how many reviews have a polarity score above zero. Is that a good definition of "positive"? What are two ways this threshold could mislead Priya?

> *Think about reviews that score 0.01 vs 0.95. Are they equally "positive"?*

**Q4 — Priya's real question.**
The notebook ends with Priya asking *"Could it be 50% if we just had a different week of reviews?"* Why is this a harder question than it sounds? What would you need to know to answer it properly?

> **Sample angle (check after you try):** Sarah ran her model on one particular batch — reviews from one week. A different week might have had a slightly different mix of customers, more complaints after a sale, or just random variation. The "true" positive rate — across all NorthStar customers, past and future — is something we can never measure directly. Priya is asking whether Sarah's number is reliable as an *estimate* of that true rate. That gap between a sample measurement and a true population value is exactly what statistics is designed to handle. L02 is how Sarah closes it.

**Q5 — What would "defending" the number look like?**
If Sarah had to stand in front of Marcus in the board room and defend "60% positive," what additional information would make her case more convincing? Write one or two things.

---

## Step 3 — Learn (30 min)

Now read about what's actually going on.

### Part A — Distributions and the normal curve (10 min)

**Watch:** [*Normal Distribution, clearly explained*](https://www.youtube.com/watch?v=rzFX5NWojp0) (StatQuest, ~5 min)

If that link is unavailable, search YouTube for "StatQuest Normal Distribution" and watch the first result.

**While you watch, hold one question in mind:** *Why does the shape of a distribution matter for deciding which summary statistic to use?*

### Part B — The Central Limit Theorem and confidence intervals (10 min)

**Watch:** [*Confidence Intervals, clearly explained*](https://www.youtube.com/watch?v=TqOeMYtOc1w) (StatQuest, ~8 min)

This video introduces both the CLT (why sample averages behave nicely) and confidence intervals (the bracket around an estimate).

**Watch at 1.0× speed** — these concepts are worth hearing slowly, especially the part where Josh distinguishes what a 95% CI *does* and *doesn't* say.

### Part C — Preview the concept reference (5 min)

Open [`lesson.md`](./lesson.md). Read the following sections only:

- "The bridge from L01" (top of the file — sets the problem)
- "Why Probability & Statistics matters for ML" (the three bullets)
- "Part 1 — What is a distribution?" through "Normal vs skewed distributions" only
- "Part 2 — The Central Limit Theorem" (the analogy paragraph — read it twice)

**Do not try to read the whole document now.** Skim, don't study — the Part notebooks will walk you through every concept with code. You just need a mental map.

### Part D — Peek at the Part notebooks (5 min)

Open each Part notebook and read just the first markdown cell — the business scenario — then close it.

| Notebook | One-sentence preview |
|---|---|
| `notebooks/02_distributions.ipynb` | Sarah plots NorthStar review data and learns to read the shape. |
| `notebooks/03_confidence_intervals.ipynb` | Sarah hand-labels 200 reviews and builds a confidence bracket around her accuracy. |
| `notebooks/04_ab_testing.ipynb` | Aisha proposes a coupon; Sarah designs the experiment that tells Marcus whether it works. |

---

## Step 4 — Practise (20 min)

Three short exercises. Attempt each one *before* looking at the sample answer. The act of trying before looking is what makes the learning stick.

---

### Exercise 1 — Mean, median, and shape (~7 min)

Below are five datasets. Without calculating anything, predict:
- Whether the mean or median would be *higher*
- Whether you'd describe the distribution as left-skewed, right-skewed, or roughly symmetric

| Dataset | What it measures |
|---|---|
| A | Monthly salaries of 50 employees at a small company (most earn £30k–£50k; the CEO earns £800k) |
| B | Heights of 1,000 adult women in the UK |
| C | Number of social media followers for 10,000 randomly chosen accounts |
| D | Test scores on a very easy exam (most people scored between 85 and 100 out of 100) |
| E | Daily coffee shop revenue over one year (the shop closes on two days for emergency repairs) |

*Write your answers before reading below.*

> **Sample answers:**
>
> **A** — Mean higher than median. Right-skewed. The CEO's £800k salary pulls the mean upward; the median (the middle earner) is far more representative. Classic salary distribution.
>
> **B** — Mean ≈ median. Roughly symmetric (normal). Height is one of the textbook normal-distribution examples — most women cluster near the average, with a smooth drop-off on each side.
>
> **C** — Mean higher than median. Strongly right-skewed. Most accounts have few hundred followers; a tiny number of celebrities have millions. This is the most extreme example — social media follower counts are often described as a "power law" or "Pareto" distribution.
>
> **D** — Mean lower than median. Left-skewed. When most values cluster near the top and the tail stretches downward (a few low scorers), the mean is pulled below the median. Also called "negatively skewed."
>
> **E** — Mean slightly lower than median. Mildly left-skewed. Two days of zero (or near-zero) revenue pull the mean down. The median (ignoring those outliers) is probably close to a typical day's revenue. A small effect, but noticeable.

---

### Exercise 2 — Confidence interval interpretation (~7 min)

Three statements about confidence intervals. For each, say whether it is correct or incorrect — and if incorrect, write the corrected version.

**Statement A:** "A 95% confidence interval means there is a 95% chance the true value is inside this specific interval."

**Statement B:** "A 95% confidence interval computed from 2,000 samples will be narrower than the same interval computed from 100 samples."

**Statement C:** "If a 95% confidence interval for a model's accuracy is [72%, 88%], you can be confident the model's accuracy is at least 72%."

*Write your answers before reading below.*

> **Sample answers:**
>
> **A — Incorrect.** The 95% is a property of the *method*, not this specific interval. The true value is a fixed (if unknown) number — it's either in the interval or it isn't. The correct interpretation: "If we repeated this study many times, 95% of the intervals we computed would contain the true value." The interval in front of you is just one of those many possible intervals.
>
> **B — Correct.** The interval width is proportional to 1/√n. With 2,000 samples the width is roughly √(100/2000) = ~22% of the 100-sample width. More data → more certainty → narrower interval.
>
> **C — Mostly correct in plain-English business communication.** You can say "the interval [72%, 88%] suggests the model's accuracy is likely above 72%." That's a reasonable everyday summary. The technical caveat: a confidence interval is a property of the *method* (95% of similar intervals would contain the truth), not a probability statement about *this specific* interval. For a published paper or a regulator, avoid wording like "X% probability that the truth is at least 72%" — that requires a Bayesian framework, not the standard frequentist CI. For everyday business reports, "likely above 72%" is fine.

---

### Exercise 3 — P-value intuition (~6 min)

An A/B test is run on NorthStar's checkout page. Version A is the original. Version B has a new "Sustainability Promise" badge. Conversion rate (orders / visitors) is tracked.

**Results after 2 weeks:**
- Version A: 5.2% conversion, 3,000 visitors
- Version B: 5.8% conversion, 3,000 visitors
- p-value: 0.031

Answer these questions:

1. Is the result statistically significant at the conventional 0.05 threshold?
2. Your colleague says: "p = 0.031 means there's a 3.1% probability the badge doesn't actually help." Is this correct?
3. Should NorthStar immediately roll out the badge to all customers? Why or why not?

*Write your answers before reading below.*

> **Sample answers:**
>
> **1.** Yes — 0.031 < 0.05, so the result is statistically significant. The difference in conversion rates is unlikely to have occurred by chance alone.
>
> **2.** No — same common mis-reading as Exercise 2A. The p-value is the probability of seeing a difference this large *if the badge had zero effect*. It is not the probability of the null hypothesis being true. The correct reading: "A 0.6pp improvement would happen by chance alone only 3.1% of the time if the badge had no effect."
>
> **3.** Not necessarily on statistics alone. The 0.6 percentage point improvement is real — but the business question is whether it is *worth it*. How much does the badge implementation cost? Does it slow page load time (which could hurt conversion elsewhere)? Does the improvement persist, or is it a novelty effect that fades? Statistical significance tells you the effect is real. It does not tell you the effect is large enough to act on. Always pair a p-value with an effect size and a business case.

---

## Bring to the Session

Come ready with:

1. **Your answer to Step 2, Q4** — what you would need to know to properly answer Priya's "different week" question. Be ready to compare with classmates.
2. **Your answer to Exercise 3, Question 3** — should NorthStar roll out the badge? Be prepared to argue for or against.
3. **One real-world number from your own work or life** where you've been given a percentage or average without any indication of how reliable it is. Be ready to say what a confidence interval would have added.
4. **One concept that didn't fully click.** The session will address it.

You do not need to have read all of `lesson.md` — the instructor will walk you through it in class. You just need to have run `01_monday_morning.ipynb` and attempted the three exercises.

---

## If You Ran Out of Time

Did the full 75 minutes but haven't finished everything? Prioritise in this order:

1. **Run `01_monday_morning.ipynb`** — non-negotiable. The whole class builds on your having seen the data and Priya's question.
2. Attempt Exercise 1 (mean, median, shape) and Exercise 3, Question 1 only.
3. Skip the videos if you must — the instructor will cover the key ideas.

You can come back to the rest after class. The same try → reflect → learn → practise rhythm repeats in class (the Part notebooks) and once more after class (the assignment), so no single miss is fatal.
