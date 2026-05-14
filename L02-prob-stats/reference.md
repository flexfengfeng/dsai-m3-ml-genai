# Reference — L02 Probability & Statistics for Machine Learning

---

## Further Reading and Watching

Use these after completing the lesson. Start with free resources.

### Free — Videos

**StatQuest with Josh Starmer** (YouTube)
Plain-English statistics and ML, one concept per short video. The best channel on the internet for exactly this material.
- [Normal Distribution, clearly explained](https://www.youtube.com/watch?v=rzFX5NWojp0)
- [Standard Deviation](https://www.youtube.com/watch?v=SzZ6GpcfoQY)
- [The Central Limit Theorem](https://www.youtube.com/watch?v=YAlJCEDH2uY)
- [Confidence Intervals](https://www.youtube.com/watch?v=TqOeMYtOc1w)
- [P-values, clearly explained](https://www.youtube.com/watch?v=5Z9OIYA8He8)
- [How to interpret a p-value (common mistakes)](https://www.youtube.com/watch?v=KEofcJ1tfkI)

**3Blue1Brown**
Beautiful visual explanations of deeper mathematical ideas.
- [But what is the Central Limit Theorem?](https://www.youtube.com/watch?v=zeJD6dqJ5lo)

**CrashCourse Statistics**
A friendly, fast-paced series on the whole of statistics.
- [Playlist: CrashCourse Statistics](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNM_Y-bUAhblSAdWRnmBUcr)

### Free — Interactive

**Seeing Theory** (Brown University)
Beautifully animated, interactive visualisations of every concept in this lesson — distributions, CLT, confidence intervals, hypothesis testing. Highly recommended.
- https://seeing-theory.brown.edu/

**Khan Academy — Statistics and Probability**
Self-paced, beginner-friendly. Covers everything in this lesson and more.
- https://www.khanacademy.org/math/statistics-probability

**GeoGebra — Normal Distribution Explorer**
Drag sliders to change the mean and standard deviation and watch the curve update in real time.
- https://www.geogebra.org/m/NdK6K4mr

### Free — Reading

**Think Stats** — Allen B. Downey
A free, Python-forward statistics textbook written for programmers. Very readable.
- https://greenteapress.com/wp/think-stats-2e/

**OpenIntro Statistics**
Free textbook, thorough treatment of inference and hypothesis testing with clear examples.
- https://www.openintro.org/book/os/

### Books (when you want depth)

**Naked Statistics** — Charles Wheelan
No formulas, all intuition. The best book for a non-technical reader who wants to understand what statistics is actually doing. Start here.

**Statistics** — David Freedman, Robert Pisani, Roger Purves
The classic non-technical textbook. Reads like a novel; builds real statistical intuition before any formulas.

**An Introduction to Statistical Learning** — James, Witten, Hastie, Tibshirani
Free PDF. Chapter 2 (Statistical Learning) and Chapter 5 (Resampling) are especially relevant to L02 concepts.
- https://www.statlearning.com/

---

## Glossary

Key terms from this lesson, in plain English. Every term appears in at least one notebook.

**A/B Test**
An experiment where two versions of something (a message, a feature, a design) are shown to different groups simultaneously. You measure one outcome metric and use statistics to decide whether any difference is real or just random variation. Also called a randomised controlled experiment.

**Alternative Hypothesis (H₁)**
The statement you are trying to find evidence for — the "something happened" claim. In Sarah's coupon test: "sending the coupon changes churn rate." You never prove this directly; you either reject the null hypothesis or fail to.

**Bootstrapping**
A resampling technique where you draw many random samples *from your own data* (with replacement) and compute a statistic each time. The spread of those values estimates the uncertainty in your statistic — without any mathematical formula. Covered in `optional_extensions.ipynb`.

**Central Limit Theorem (CLT)**
The result that the average of many random samples will follow a normal distribution, regardless of what the original data looks like. Why it matters: it means you can use confidence intervals and p-values on almost any data, as long as your samples are large enough.

**Confidence Interval (CI)**
A range of values around an estimate that probably contains the true value. A 95% CI means: if you repeated the same experiment many times, 95% of the intervals you computed would contain the true value. It does *not* mean "95% probability that the true value is in this particular interval."

**Confidence Level**
The probability level you choose for your confidence interval — typically 95%. Higher confidence levels produce wider intervals. A 99% CI is wider than a 95% CI.

**Descriptive Statistics**
Statistics that summarise data you already have — mean, median, standard deviation, a histogram. They describe; they do not generalise beyond the data in hand.

**Distribution**
The pattern that shows how often different values appear in a dataset. A distribution tells you what "typical" looks like and how spread out the values are. Can be shown as a histogram or a smooth curve.

**Inferential Statistics**
Statistics used to make claims about a population larger than the data you measured. Confidence intervals and p-values are inferential tools — they generalise from a sample to the wider truth.

**Mean**
The average value: add all values, divide by the count. The mean is pulled toward extreme values, so it is misleading when the data is skewed.

**Median**
The middle value when data is sorted. The median is resistant to extreme values (outliers), so it is often more representative of a "typical" case than the mean.

**Normal Distribution**
The symmetric, bell-shaped curve. Most values cluster near the mean; the further from the mean, the rarer. Many natural measurements (heights, measurement errors, test scores) follow this shape. Many ML algorithms assume data is roughly normally distributed.

**Null Hypothesis (H₀)**
The boring default: "nothing happened," "there is no difference," "the intervention had no effect." You never prove the null hypothesis; you either find evidence against it (small p-value) or fail to (large p-value).

**Outlier**
A data point that sits far from the rest of the distribution. Outliers are detected using Z-scores; they may be errors, or they may be the most interesting data you have.

**P-value**
The probability of seeing a result at least as extreme as yours *if the null hypothesis were true*. A small p-value means your result would be very unlikely by chance alone — so you reject the null hypothesis. A p-value is not the probability that your hypothesis is true.

**Polarity Score**
A number between −1 (fully negative) and +1 (fully positive) that a sentiment model outputs. Used in the L02 notebooks as the raw data Sarah analyses.

**Sampling**
Taking a smaller group from a larger population to estimate something about the whole. Random sampling ensures the smaller group is representative.

**Skewed Distribution**
A distribution where one tail stretches much further than the other. Right-skewed (positive skew): most values are low but a long tail extends to the right — typical of incomes, web traffic, and complaint counts. Left-skewed: most values are high with a long tail to the left.

**Standard Deviation**
A one-number summary of how spread out the data is. In a normal distribution, roughly 68% of values fall within one standard deviation of the mean, and 95% fall within two.

**Statistical Significance**
A result is statistically significant if its p-value is below a chosen threshold (typically 0.05). This means the result is unlikely to have occurred by chance — it does not mean the result is large, important, or practically useful.

**T-test**
A statistical test that compares the means of two groups and produces a p-value. It answers: "Could this difference in averages have happened by chance, or is it real?" Covered in `optional_extensions.ipynb`.

**Type I Error (False Positive)**
Rejecting the null hypothesis when it is actually true — concluding there is an effect when there isn't one. The p-value threshold (e.g. 0.05) directly controls the rate of Type I errors.

**Type II Error (False Negative)**
Failing to reject the null hypothesis when there is actually a real effect — missing a signal that's there. Reduced by using larger samples.

**Z-score**
A standardised measure of how far a value sits from the mean, expressed in units of standard deviation. Formula: `z = (value − mean) / standard deviation`. A Z-score of 2 means "two standard deviations above average" — unusual but not extreme. A Z-score of 3.5 means "very unusual, worth investigating."

---

## Cast of Characters — L02

**Sarah Chen** — Customer Experience Analyst, NorthStar Retail. Our protagonist across all 10 lessons. In L02 she learns to defend numbers, not just produce them.

**Priya Raman** — CMO and Sarah's manager. The one who asked *"How sure are we?"* at the end of L01, triggering this whole lesson. Her recurring pressure — *"Show me the money, not the accuracy"* — shapes how every future model is evaluated.

**Aisha Patel** — Head of Customer Service. She proposes the apology coupon in Thursday's A/B test scenario. A pragmatic optimist who wants results fast.

**Marcus Wong** — CTO. Makes his first speaking appearance in L02's Thursday scene, asking whether the apology coupon will actually reduce churn or just add operational complexity for noise. He is the reason every result needs a p-value. His trust has to be earned (an early model failure made him cautious).

**Tom Bradley** — Head of Analytics at Lakeside Bank, NorthStar's banking partner. Returns for the L02 assignment. His question (does the new onboarding flow reduce complaints?) uses all three statistical lenses you learn this week.

---

## Want to go further?

- **Seeing Theory** (https://seeing-theory.brown.edu/) is the fastest way to build visual intuition for everything in this lesson
- The **StatQuest** playlist above covers every concept in L02 in 5–15 minute videos
- After this course, Andrew Ng's [Machine Learning Specialization on Coursera](https://www.coursera.org/specializations/machine-learning-introduction) goes deeper on the statistical foundations
