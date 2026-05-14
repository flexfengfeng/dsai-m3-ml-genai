# Lesson — L02 Probability & Statistics for Machine Learning

> **Chapter 2 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · NorthStar Retail · January 2023.*
> On Friday afternoon, Priya said: *"Your model says 60% of reviews are positive. But are the positive ones actually positive? How do we know we can trust that number?"*
> Sarah didn't have an answer. This lesson is how she gets one.

Use this document as your concept reference — before, during, and after the session. Each section explains a key idea in plain English, anchors it to Sarah's week at NorthStar, and shows why it matters for the rest of the course.

| Section | Notebook | Sarah's day | Time |
|---|---|---|---|
| Part 1: Distributions | `notebooks/02_distributions.ipynb` | Tuesday | ~60 min |
| Part 2: The CLT + Confidence Intervals | `notebooks/03_confidence_intervals.ipynb` | Wednesday | ~60 min |
| Part 3: A/B Testing + P-values | `notebooks/04_ab_testing.ipynb` | Thursday | ~60 min |
| Check your understanding | at the end of this document | — | ~20 min |

---

## The bridge from L01

In L01, Sarah ran a pre-trained sentiment model on 10,000 NorthStar reviews and reported: *"60% positive, 40% negative."* The model ran in seconds. The result looked confident.

Then Priya asked the question that statistics exists to answer: **"Could it have been 50% if we just had a different week of reviews?"**

Sarah's number is a *sample statistic* — computed from one particular batch of data. The true sentiment rate across all NorthStar customers (past, present, future) is a *population parameter* that nobody can measure directly. Probability and statistics are the tools that connect the two: they let Sarah say how far off her sample number is likely to be, and how confident she can be in any claim she makes from it.

---

## Why Probability & Statistics matters for Machine Learning

You will use these ideas in every ML lesson that follows.

**Distributions tell you what "normal" looks like** — before you can detect an anomaly, you need a baseline. Before you can evaluate a model's error, you need to understand the spread of the data. Every feature you feed to a model has a distribution, and that shape affects how the model trains and how its outputs should be interpreted.

**Confidence intervals tell you how uncertain your estimates are** — accuracy of 84% sounds precise. But 84% of *what sample*? From how many examples? A confidence interval is the honest version: "between 78% and 89%, with 95% confidence." This is the language of any serious ML evaluation report.

**P-values and A/B tests tell you whether interventions actually work** — once you have a model, the next question is always "does acting on this model's output change anything?" The A/B test is how you answer that rigorously, without fooling yourself into seeing patterns that are just random variation.

---

## Part 1: Distributions

### What is a distribution?

**The idea in plain English:** A distribution is the pattern of how often each value appears in a dataset. Plot every review's polarity score on a number line and mark how many reviews land at each value — the shape that emerges is the distribution.

**Real-world analogy:** Think of a coffee shop's daily customer count. Some days are quiet (40 customers), some are busy (200 customers), but most are somewhere in the middle. If you drew a histogram of every day's count over a year, you'd see a shape — maybe a bell curve, maybe a lopsided one. That shape is the distribution of daily footfall. It tells you what "a normal day" looks like, and which days were unusual.

**Why it matters for ML:** Most ML algorithms make assumptions about the distributions of the features they receive. A linear model assumes a roughly normal spread. Tree-based models are distribution-agnostic but still affected by extreme outliers. Knowing the distribution helps you decide whether to apply transformations (like log-scaling skewed data) before training.

---

### Normal vs skewed distributions

**The idea in plain English:** A *normal* (bell-curve) distribution is symmetric — the mean and median are nearly equal, and values thin out evenly on both sides. A *skewed* distribution leans in one direction — there are more extreme values on one side than the other, pulling the mean away from the median.

**Real-world analogy:** Heights of adult men in a country follow a roughly normal distribution — most men are somewhere near the average, with roughly equal numbers a bit shorter and a bit taller. Annual household incomes follow a right-skewed distribution — most households earn a moderate amount, but a long tail of very high earners pulls the mean upward. The median income is typically thousands of pounds below the mean, because the average is pulled by the wealthy tail.

This is why headlines say "average household income rose" while many families feel no improvement — they're seeing the median, not the mean.

**Why it matters for ML:** Skewed features can mislead algorithms. A small number of extreme values can dominate the model's learning. Recognising skew is the first step to deciding whether to apply a log transformation or winsorise outliers before training. Sarah's polarity scores are mildly skewed — something the `02_distributions.ipynb` notebook reveals the moment she plots them.

---

### Z-scores: measuring how unusual a value is

**The idea in plain English:** A Z-score answers the question: *"How many standard deviations away from the average is this value?"* A Z-score of 0 means exactly average. A Z-score of 2 means "two standard deviations above average" — unusual. A Z-score of -3 means "three standard deviations below average" — very unusual.

Formula: `z = (value − mean) / standard deviation`

**Real-world analogy:** Imagine you score 85 on an exam. Is that good? It depends on how everyone else did. If the class average was 60 and the standard deviation was 10, your Z-score is (85−60)/10 = 2.5 — you're in the top 1% of the class. If the average was 82 and the standard deviation was 2, your Z-score is 1.5 — still good, but not exceptional. The raw score means nothing without context; the Z-score provides that context automatically.

**Why it matters for ML:** Z-scores are the foundation of feature scaling (standardisation) — one of the most common data preprocessing steps before training. They are also how anomaly detection works: flag any value with |Z| > 3 as an outlier worth investigating.

---

### Quick Check — Part 1

**Q1:** Sarah plots the polarity scores for 10,000 reviews and finds the mean is 0.12 and the median is 0.06. What does this tell her about the distribution's shape?

> **Sample answer:** The mean (0.12) is higher than the median (0.06), which means the distribution is right-skewed — a long tail of very positive reviews is pulling the mean upward. The median better represents the "typical" review. Sarah should report the median sentiment when describing a "typical" review, and be explicit that a small number of very positive reviews are inflating the mean.

**Q2:** A review has a polarity score of 0.92. The dataset's mean polarity is 0.12 and the standard deviation is 0.36. Calculate the Z-score and interpret it.

> **Sample answer:** Z = (0.92 − 0.12) / 0.36 = 0.80 / 0.36 ≈ 2.22. This review is about 2.2 standard deviations above the mean — positive, and a bit unusual, but not extreme. It would not be flagged as a statistical outlier (the typical cut-off is |Z| > 3).

**Q3:** Priya asks whether the distribution of review lengths is likely to be normal or skewed. Without running any code, what would you predict — and why?

> **Sample answer:** Right-skewed. Most reviews are probably short (a few words or a sentence or two), but a small number of very detailed complaints or essays will stretch the tail to the right. Text length in user-generated content is almost always right-skewed. The same pattern appears in social media posts, emails, and support tickets.

---

## Part 2: The Central Limit Theorem + Confidence Intervals

### Why sampling works

**The idea in plain English:** Nobody can measure every customer, every patient, or every transaction. Instead, we measure a *sample* — a smaller group selected to be representative of the whole. The big question is: how much can we trust a sample statistic (like "84% accuracy on 200 reviews") as an estimate of the true value (accuracy on all reviews ever)?

**Real-world analogy:** Before an election, pollsters can't ask every voter. They ask a carefully chosen sample of a few thousand, and they report the result with a margin of error: "Party A leads with 43%, ± 3 percentage points." That margin of error comes from exactly the mathematics you are learning here.

**Why it matters for ML:** Every model evaluation is a sampling problem. You evaluate your model on a test set — a sample. The accuracy number you get is a sample statistic. The real accuracy, on all possible future data, is a population parameter. Confidence intervals are how you communicate the uncertainty honestly.

---

### The Central Limit Theorem

**The idea in plain English:** Here is the most important result in statistics for machine learning: *even if the underlying data is not normally distributed, the average of many samples from that data will follow a normal distribution.* The larger the sample, the more normal the distribution of sample means becomes.

**Real-world analogy:** Imagine a jar of Skittles with different colours in very unequal proportions — not normal at all. Now take a handful of 30 Skittles, count the red ones, put them back, repeat 1,000 times. Plot the count of red Skittles per handful. Even though the jar's proportions are lopsided, the distribution of your handful counts will look like a bell curve. That is the CLT at work.

**Why it matters for ML:** The CLT is why so many ML evaluation techniques (confidence intervals, t-tests, hypothesis tests) work in practice on messy, non-normal data. As long as your sample is large enough, you can use normal-distribution-based mathematics even when the underlying data is skewed, bimodal, or oddly shaped.

*The rule of thumb:* samples of 30+ are usually enough for the CLT to apply. The `03_confidence_intervals.ipynb` notebook shows this visually by resampling Sarah's labelled-review accuracy 1,000 times.

---

### Confidence intervals

**The idea in plain English:** Sarah hand-labels 200 reviews and computes 84% accuracy. But 200 is a small sample. If she had labelled a different 200, she might have gotten 81% or 87%. A confidence interval quantifies that range: "based on this sample, the true accuracy is likely between 78% and 89%."

More precisely: if you ran this labelling exercise many, many times with different samples of 200, 95% of the confidence intervals you computed would contain the true accuracy. That's what "95% confidence" means.

**The formula (approximate, for proportions):**

```
CI = p̂  ±  1.96 × sqrt( p̂(1 − p̂) / n )
```

where p̂ is your sample proportion (0.84), n is your sample size (200), and 1.96 is the Z-score for 95% confidence.

**Real-world analogy:** Polling again. "43% ± 3 points" is a 95% confidence interval. The pollster is saying: "if we ran this poll a hundred more times, 95 of them would give a result within 3 points of 43%." The remaining 5 polls would fall outside — not because the poll was wrong, but because random sampling sometimes produces unusual results.

**The most common mis-reading:** *"There is a 95% probability that the true accuracy is in this interval."* This sounds right but is subtly wrong. The true accuracy is a fixed (if unknown) number — it doesn't have a probability of being in any range. What has the 95% probability is the *method*: 95% of intervals computed this way will capture the true value. This is a fine distinction, but it matters in L03 when you need to be precise about what your model evaluation is actually claiming.

**Why it matters for ML:** Any time you report a model's accuracy, precision, or recall, you should report it with a confidence interval. A model with 84% (95% CI: 78–89%) is saying something very different from a model with 84% (95% CI: 83–85%).

---

### Quick Check — Part 2

**Q1:** Sarah computes a 95% confidence interval of [78%, 89%] for her model's accuracy. Her colleague says: "So there's a 95% chance the real accuracy is in that range." Is the colleague right? Why or why not?

> **Sample answer:** Not quite. The 95% refers to the method, not this specific interval. The true accuracy is a fixed (if unknown) number. A better statement is: "If we repeated this labelling exercise many times with different samples of 200 reviews, 95% of the intervals we computed would contain the true accuracy." It's a subtle distinction, but it matters: you cannot assign a probability to whether one specific interval contains the truth.

**Q2:** Sarah wants a narrower confidence interval. She currently has n=200 reviews labelled. What should she do — and roughly how much more labelling effort will she need to halve the interval width?

> **Sample answer:** She should label more reviews. The interval width is proportional to 1/√n, so to halve the width she needs to quadruple the sample size: from 200 to roughly 800 reviews. This illustrates the "diminishing returns" of sampling — each additional reduction in uncertainty requires exponentially more data.

**Q3:** The Central Limit Theorem says sample means follow a normal distribution regardless of the underlying data's shape. Why does this make it safe to use confidence intervals even when your data is skewed?

> **Sample answer:** Because the confidence interval formula uses the distribution of the *sample mean* (or proportion), not the distribution of the raw data. Even if individual polarity scores are skewed, the distribution of the average polarity across many different samples will be approximately normal — that's the CLT. So the formula (which assumes normality) still gives reliable results, as long as the sample is large enough (rule of thumb: n ≥ 30).

---

## Part 3: A/B Testing + P-values

### Designing an A/B test

**The idea in plain English:** An A/B test is an experiment. You take two groups — a *control group* that keeps doing things the old way, and a *treatment group* that experiences the change — and you compare one specific outcome metric between them. The goal is to decide: is any difference in the metric real, or could it have happened by random chance?

**Real-world analogy:** A supermarket wants to know whether putting fruit near the checkout increases healthy snack purchases. They redesign half their stores (treatment) and leave the other half unchanged (control). After 4 weeks, they compare the purchase rates. That's an A/B test. The decision about whether the difference is "real" requires statistics — because random store-to-store variation could explain a small difference even if fruit placement does nothing.

**The four components you must define before running a test:**
1. **Control group** — the group that does not receive the intervention
2. **Treatment group** — the group that receives the intervention
3. **Outcome metric** — the one number that defines success (keep it singular; testing multiple metrics at once inflates false discoveries)
4. **Sample size** — how many observations per group before you stop and decide

In Sarah's Thursday test: control = negative-flagged customers who get no coupon; treatment = negative-flagged customers who receive an apology coupon; metric = first-30-days complaint rate; sample = determined in the notebook.

---

### The p-value

**The idea in plain English:** The p-value answers a specific question: *"If the intervention had zero effect, what is the probability of seeing a difference at least this large, purely by chance?"*

A small p-value (typically below 0.05) means: this result would be very unlikely by chance alone, so we reject the "no effect" hypothesis. A large p-value means: this result is easily explained by random variation; we don't have enough evidence to claim the intervention worked.

**Real-world analogy:** You flip a coin 20 times and get 16 heads. Is the coin biased, or did you just get lucky? The p-value is exactly the probability of getting 16 or more heads on a fair coin. If that probability is very small (it's about 0.6%), you conclude the coin is probably biased. If the probability were high (say, 30%), you'd shrug and say "could easily be chance."

**Why it matters for ML:** Every time you A/B test a deployed model's output — does sending the model's recommended email improve conversion? Does flagging the model's high-risk customers reduce churn? — you need a p-value to tell you whether the effect is real. Without it, you're just telling stories.

---

### The three most common mis-readings of a p-value

These mistakes appear constantly in business, journalism, and even some published research. Know them so you don't repeat them.

**Mis-reading 1:** *"p = 0.03 means there is a 3% probability that the null hypothesis is true."*

**The truth:** The p-value is the probability of the *data given the null hypothesis* — not the probability of the *null hypothesis given the data*. These are completely different. (The probability of the null hypothesis requires a Bayesian framework; see `optional_extensions.ipynb`.)

**Mis-reading 2:** *"p = 0.04, which is significant — so the effect is large and important."*

**The truth:** Statistical significance and practical significance are separate. With a large enough sample, even a tiny, meaningless difference will produce a p-value below 0.05. Always report the *effect size* (how large was the actual difference?) alongside the p-value.

**Mis-reading 3:** *"p = 0.06 — the result is not significant, so the intervention had no effect."*

**The truth:** A p-value above 0.05 means you lack *sufficient evidence* to reject the null hypothesis — not that the null hypothesis is true. It might mean the effect is real but your sample was too small to detect it. "Absence of evidence is not evidence of absence."

---

### Quick Check — Part 3

**Q1:** Sarah runs an A/B test on Aisha's apology coupon. The p-value is 0.038. Marcus asks: "Does this mean there's only a 3.8% chance the coupon didn't work?" How should Sarah correct him?

> **Sample answer:** Gently. The p-value of 0.038 means: if the coupon had zero effect, there is only a 3.8% probability of seeing a difference at least this large by random chance. Since this is below the 0.05 threshold, Sarah rejects the null hypothesis — the evidence suggests the coupon has a real effect. But the p-value does not directly tell us the probability that the null hypothesis is true or false. That requires a different framework (Bayesian inference). For business purposes, the practical message is: "We have strong statistical evidence the coupon reduced complaints; based on this test, we recommend rolling it out."

**Q2:** *Imagine a different version of Sarah's experiment, run on a much larger sample.* The coupon group had a complaint rate of 12.1% vs the control group's 13.0% — a 0.9 percentage point difference, p = 0.002. Should NorthStar roll out the coupon to all customers immediately?

> **Sample answer:** Not necessarily. A 0.9 percentage point reduction is *statistically* significant (very low p-value, because the sample is huge) but may not be *practically* significant. The business question is: does the revenue from reduced churn outweigh the cost of the coupons? A 0.9pp reduction on, say, 10,000 at-risk customers is 90 fewer complaints — is the value of those 90 retained customers worth more than the coupon spend? Marcus's instinct to ask "will this actually reduce churn or just cost us money?" is exactly the right framing. (This is *why* effect size matters: with enough data, almost any non-zero effect becomes "significant," and you need an outside-statistics judgement to decide whether to act.)

**Q3:** A colleague runs a test with 50 customers per group, finds p = 0.08, and concludes "the coupon doesn't work." What might be wrong with this conclusion?

> **Sample answer:** With only 50 customers per group, the test is underpowered — the sample size is too small to reliably detect real effects. A 0.08 p-value means there's insufficient evidence to reject the null hypothesis, but that's not the same as evidence of no effect. The real effect could be real but undetectable at this sample size. The colleague should increase the sample size (often hundreds per group are needed for a small effect) before concluding anything. "We couldn't detect an effect" is very different from "the coupon doesn't work."

---

## Putting it all together

Sarah's Friday presentation uses all three tools in sequence, and they form a natural chain.

**Distributions first.** Before you can interpret any number, you need to understand the shape of the data it came from. Skewed polarity scores mean the mean overstates "positive" sentiment. Sarah leads with the histogram so the audience understands what the raw data looks like.

**Confidence intervals second.** Sarah's model accuracy of 84% is a point estimate from a sample. The confidence interval (78–89%) is the honest version — it tells Priya the range of accuracy the model is likely to have in the real world, not just on 200 hand-labelled reviews.

**A/B test and p-value third.** Now that the model is trusted (within its CI), the question becomes "should we act on it?" The A/B test on Aisha's apology coupon is the answer. Sarah reports the effect (0.9pp reduction in complaints) and the statistical confidence (p < 0.05).

In every ML project you will run in this course, these three steps appear in some form. L03 adds the tools to build models from labelled data; L04 adds feature engineering and evaluation depth. But the statistical vocabulary you built today is the lens through which every later model is judged.

---

## Check your understanding

Work through these after finishing the three Part notebooks. Attempt each question on your own first.

### Part 1 — Distributions

**Q1 — Which summary stat?** NorthStar's analysts report that the "average spend per order" is £85. You look at the data and notice the distribution is heavily right-skewed (a small number of very large corporate orders). Is £85 the right number to report to Priya? What would you suggest instead?

> **Sample answer:** No. Right-skewed data means a few large orders are pulling the mean up, making it unrepresentative of a typical customer. Report the *median spend per order* instead — it is resistant to those large outliers. Also consider segmenting: report the median for individual customers separately from corporate accounts. Give Priya both numbers with a sentence explaining why they differ.

**Q2 — Z-score intuition.** A product's daily return rate has a mean of 3.2% and a standard deviation of 0.8%. Today's return rate is 5.6%. Is this an anomaly worth investigating?

> **Sample answer:** Z = (5.6 − 3.2) / 0.8 = 3.0. A Z-score of 3.0 means today's rate is 3 standard deviations above the mean. In a normal distribution, only about 0.27% of days would fall this far out by chance — roughly 1 day per year. Yes, this is worth investigating: possible product defect, batch quality issue, or data error.

**Q3 — Distribution shape and the model.** You are training a fraud detection model. The transaction amounts in your dataset are extremely right-skewed (99% of transactions are under £500; 1% are very large). Why might this be a problem for training, and what could you do?

> **Sample answer:** The model may treat a £50,000 transaction as an extreme outlier in ways that distort learning — depending on the algorithm, one or two very large values can dominate the loss function. Common solutions: apply a log transformation to the amount (making the distribution more symmetric), or winsorise the data (cap extreme values at a threshold like the 99th percentile). Either approach reduces the influence of the long tail without discarding those transactions.

### Part 2 — Confidence Intervals

**Q4 — Narrow vs wide.** A model evaluated on 50 reviews has 80% accuracy (95% CI: 68–92%). Another model evaluated on 2,000 reviews also has 80% accuracy (95% CI: 78–82%). Which model evaluation do you trust more, and why?

> **Sample answer:** The second model's evaluation — not because the accuracy is different (both are 80%), but because the confidence interval is much narrower. The first CI (68–92%) spans 24 percentage points and includes both "barely acceptable" and "genuinely excellent" as possibilities. The second CI (78–82%) is tight enough to be useful for decisions. The only difference is sample size: 50 vs 2,000. This is why you should always report CIs alongside accuracy.

**Q5 — CLT application.** A colleague says: "The CLT only works if the original data is normally distributed." Is this correct?

> **Sample answer:** No — this is backwards. The CLT says that sample means approach a normal distribution regardless of the shape of the underlying data. The underlying data can be skewed, bimodal, or whatever else — as long as your samples are large enough (n ≥ 30 is the rule of thumb), the distribution of sample means will be approximately normal. That is precisely what makes the CLT so powerful.

**Q6 — Interpretation.** Sarah reports "84% (95% CI: 78–89%)." Priya asks: "If we ran this test again with 200 new reviews, would we get exactly 84% again?" How should Sarah answer?

> **Sample answer:** Probably not. The CI (78–89%) is Sarah's way of saying: with different samples, the accuracy would probably fall somewhere in this range. If Priya ran the test 100 times with different samples of 200 reviews, she'd expect about 95 of those tests to produce a CI that contains the true accuracy — and the point estimates would vary. The 84% is the best estimate from this particular sample; the CI tells her how much to trust it.

### Part 3 — A/B Testing

**Q7 — Design the test.** NorthStar wants to test whether a new "easy returns" banner on the product page reduces returns (by setting better expectations before purchase). Design the A/B test: what are the control and treatment groups? What is the outcome metric? What would make you conclude the banner "works"?

> **Sample answer:** Control group: customers who see the product page without the banner. Treatment group: customers randomly assigned to see the product page with the "easy returns" banner. Outcome metric: return rate within 30 days of purchase (one metric — not revenue, satisfaction, and returns simultaneously). Conclusion: the banner "works" if the treatment group's return rate is lower than the control group's, with a p-value below 0.05. Also report the effect size: a 0.1pp reduction vs a 3pp reduction have very different business implications even if both are significant.

**Q8 — p-value quiz.** An A/B test produces p = 0.001. A colleague says "this is very statistically significant, so we should definitely roll out the change." What question should you ask before agreeing?

> **Sample answer:** "What is the actual size of the effect?" A p-value of 0.001 with a very large sample might correspond to a 0.01% difference in conversion rate — statistically rock-solid but practically worthless. Before rolling out, ask: how large was the measured improvement? Does it justify the development cost and any risks of the change? Statistical significance does not imply business significance.

**Q9 — Mis-reading check.** An experiment produces p = 0.12. Your manager says "the null hypothesis is confirmed — there's no effect." Correct their interpretation.

> **Sample answer:** The p-value of 0.12 means we failed to reject the null hypothesis — we don't have sufficient statistical evidence of an effect. But failure to find evidence is not evidence of no effect. The sample may have been too small, or the effect too small to detect at this sample size. The correct statement is: "We did not find statistically significant evidence of an effect at the 0.05 level. We cannot conclude the intervention does nothing — we conclude only that this test was not powerful enough to detect it."

**Q10 — End-to-end.** Sarah's Friday presentation has three numbers: (a) 60% sentiment positive rate, (b) 84% model accuracy (95% CI: 78–89%), (c) coupon A/B test p = 0.038. For each number, write one sentence that correctly interprets it for a non-technical executive audience.

> **Sample answer:**
> **(a)** "In this week's sample of 10,000 reviews, 60% were classified as positive — though this figure should be taken as an estimate, not a precise count, because the model occasionally misclassifies borderline reviews."
> **(b)** "When we checked the model against 200 reviews labelled by hand, it was correct 84% of the time; our statistical analysis suggests the true accuracy is most likely somewhere between 78% and 89%."
> **(c)** "We found strong statistical evidence that the apology coupon reduces complaint rates — results this large would occur by chance less than 4% of the time if the coupon had no effect — so we recommend proceeding with a cautious roll-out."

---

## Sarah's question for L03 (the bridge forward)

By Friday afternoon, Sarah has defended three numbers: a sentiment rate, an accuracy interval, and an A/B test result. Priya nods. Marcus nods. The room is quiet for a moment, and then Marcus says:

> *"OK, the sentiment model holds up. But you used a pre-trained one — off the shelf. Can you build us a model that predicts churn from our own customer data? Something we actually trained on NorthStar behaviour?"*

Sarah doesn't have an answer today. That question — *can I build a model?* — is the engine of **L03 (Supervised Learning)**. Come to L03 ready to train your first model from scratch.
