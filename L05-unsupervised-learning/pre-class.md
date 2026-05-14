# Pre-class — L05 Unsupervised Learning

**Time:** about 75 minutes. Do this before the live session.

**Goal:** Understand what unsupervised methods actually do and arrive ready to use them on NorthStar's customer data.

---

## What you're walking into

End of L04, Marcus said:

> *"Now — what about the customers who DON'T churn but also don't log in for a year? Can we find natural CLUSTERS of customer behaviour without labels?"*

This week Sarah has the same NorthStar customer dataset she's been working with since L01 — BUT she's dropping the `churned` column. She has to find structure without knowing the answer in advance.

By Friday she has to:
1. Reduce the 10-feature data to something visualisable.
2. Group customers into natural segments she can name.
3. Flag a "watch list" of unusual customers worth investigating.

---

## Task 1 (~20 min) — Open `01_monday_morning.ipynb`

This notebook drops the `churned` column and explores the remaining features. You see what "unlabelled data" actually looks like — you have features but no answer.

**Open** `notebooks/01_monday_morning.ipynb` and **run every cell**.

As you go, jot down:
- The number of features Sarah has to work with
- Which pairs of features look correlated
- One row that looks "unusual" to you (gut feel — you'll see if Isolation Forest agrees)

---

## Task 2 (~30 min) — Watch two videos

### Video 1 — Principal Component Analysis (PCA) (StatQuest, 22 min)

[https://www.youtube.com/watch?v=FgakZw6K1QQ](https://www.youtube.com/watch?v=FgakZw6K1QQ)

Why it matters: PCA is the single most-used dimensionality reduction technique in industry. The video walks through the math visually — watch until you understand WHAT a principal component is (not how to derive it).

**Mini-exercise:** in your own words, what does it mean when PC1 "captures 40% of variance"?

> **Sample answer:** If you project all the data points onto the PC1 axis (a single line), the spread of those projected points accounts for 40% of the total spread of the original data. PC1 is the line along which the points are most spread out. The remaining 60% of variance is in directions perpendicular to PC1.

### Video 2 — K-Means Clustering (StatQuest, 9 min)

[https://www.youtube.com/watch?v=4b5d3muPQmA](https://www.youtube.com/watch?v=4b5d3muPQmA)

Why it matters: K-Means is the most-used clustering algorithm in industry. Customer segmentation, document grouping, image colour palettes — all K-Means.

**Mini-exercise:** if you run K-Means twice on the same data with different `random_state`, will you get the same clusters?

> **Sample answer:** Not necessarily. K-Means starts by randomly choosing initial centroid positions, and the final clusters depend on where it started. With `n_init=10` (sklearn default), it tries 10 different initialisations and keeps the best, which gives more stable results — but two runs with different `random_state` can still differ slightly. ALWAYS set `random_state` for reproducibility.

---

## Task 3 (~25 min) — Quick conceptual exercises

### Exercise 1 — Choose the right technique

For each scenario, pick: PCA, K-Means, Isolation Forest, or "supervised methods from L03/L04":

1. NorthStar wants to know which customers are most likely to be fraudulent. They have 1,000 confirmed fraud cases and 100,000 confirmed non-fraud.
2. A bank wants to find unusual transactions that might be fraud. They have NO labelled examples.
3. A retailer wants to group its 10 million customers into 5 marketing segments.
4. A team has 50 features per customer and wants to visualise the data on a 2D plot.

> **Sample answers:**
> 1. Supervised (L03/L04) — labels exist; use logistic regression or gradient boosting.
> 2. Isolation Forest — no labels; flag the unusual ones.
> 3. K-Means — natural segmentation task.
> 4. PCA — reduce 50D → 2D for visualisation.

### Exercise 2 — Reading a scree plot

You run PCA and get the following `explained_variance_ratio_`:

```
PC1: 0.45
PC2: 0.22
PC3: 0.10
PC4: 0.06
PC5: 0.05
PC6: 0.04
PC7: 0.03
PC8: 0.02
PC9: 0.02
PC10: 0.01
```

How many components should you keep, and why?

> **Sample answer:** First 3 components capture 77% of variance. PCs 4–10 each add less than 6%. Reasonable answers: keep 2 (for visualisation, captures 67%), keep 3 (captures 77%, marginally better), or keep enough for ~90% variance (≈ first 5). The "right" answer depends on what you'll do with the reduced data.

### Exercise 3 — Interpret a cluster profile

K-Means gives you 4 clusters. The mean of each feature, by cluster:

| Feature | Global mean | Cluster 0 | Cluster 1 | Cluster 2 | Cluster 3 |
|---|---|---|---|---|---|
| tenure_months | 36 | 60 | 8 | 32 | 40 |
| avg_monthly_spend (£) | 70 | 160 | 30 | 65 | 75 |
| returns_per_purchase | 0.08 | 0.04 | 0.20 | 0.07 | 0.09 |
| support_tickets | 1.2 | 0.4 | 4.0 | 0.9 | 1.1 |

Name each cluster in 2-4 words.

> **Sample answers:**
> - **Cluster 0** → "Loyal high-value" — long tenure, high spend, low returns, low support tickets
> - **Cluster 1** → "New + frustrated" — short tenure, low spend, high returns AND high support
> - **Cluster 2** → "Mid-tier average" — close to the global mean on everything
> - **Cluster 3** → "Steady, normal" — close to the global mean but slightly above on tenure

---

## Active-engagement tips

- **Treat unsupervised methods as exploration**, not prediction. There's no "right answer" — only "useful answers."
- **Always look at cluster profiles** (the table above) — never just the labels. The labels alone tell you nothing.

---

## Bring to the session

1. ✅ Run `01_monday_morning.ipynb` end-to-end
2. ✅ Watched both StatQuest videos
3. ✅ Your three exercise answers ready
4. ✅ One specific question — anything that didn't click

See you Tuesday morning at Sarah's desk.
