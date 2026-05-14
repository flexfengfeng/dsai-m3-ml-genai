# Lesson 5 — Unsupervised Learning
*Concept reference. Open whenever you want to look up a definition or check the mechanics of an algorithm used in the notebooks.*

> **Where Sarah is.** Marcus's brief from end-of-L04: *"Can we find natural clusters of customer behaviour WITHOUT labels?"* This week Sarah works with the same NorthStar customer data but DROPS the `churned` column entirely. She has to find structure that wasn't labelled in advance. By Friday she has segments to present, a story for each one, and a list of "unusual" customers worth a closer look.

---

## The Big Picture — three things unsupervised learning is for

Most beginners think clustering is THE unsupervised task. It isn't. The three biggest categories of unsupervised work in industry:

1. **Dimensionality reduction.** Squeeze a 50-feature dataset into 2-10 components without losing the signal. Used for visualisation, for preprocessing, and for compression.
2. **Clustering / segmentation.** Group similar items together. Used for customer segmentation, document grouping, image colour reduction.
3. **Anomaly detection.** Flag items that look unlike everything else. Used for fraud, security threats, manufacturing defects.

L05 covers one algorithm from each: PCA, K-Means, and Isolation Forest. The three together cover ~90% of real unsupervised work.

---

## Part 1 — PCA (Principal Component Analysis)

### What it does

PCA finds new axes — called *principal components* — that capture the most variance in your data. The first principal component (PC1) is the direction along which the data is most spread out. PC2 is the direction of next-most variance, orthogonal to PC1. And so on.

If most of the variance lives in a few directions, you can drop the rest and lose very little information. That's the dimensionality reduction.

### When to use it

- **For visualisation** — project 10-dimensional customer data into 2D to see structure on a screen
- **For preprocessing** — feed a 5-component representation into a downstream model instead of 50 raw features
- **For decorrelation** — principal components are orthogonal by construction; some downstream algorithms benefit

### Mechanics in sklearn

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

# ALWAYS scale before PCA — variance is scale-dependent
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X_numeric)

pca = PCA(n_components=2)  # keep top 2 components
X_pca = pca.fit_transform(X_scaled)

print(pca.explained_variance_ratio_)
# e.g. [0.42, 0.18] → first 2 components capture 60% of total variance
```

### Reading the output

**`explained_variance_ratio_`** — what fraction of total variance each component captures. A typical pattern: the first few PCs capture most variance; the rest are noise.

**`components_`** — the coefficients that turn the original features into each PC. A large absolute value means that original feature contributes strongly to that PC. Useful for naming the components ("PC1 is mostly about *engagement*; PC2 is mostly about *spend*").

### The scree plot

Plot `explained_variance_ratio_` against component index. Look for the "elbow" where adding more components stops gaining you significant variance. Keep components up to the elbow.

### What PCA is NOT

- **It's not a labelling algorithm.** It doesn't tell you which group a point belongs to — just where it sits in the new coordinate system.
- **It's not non-linear.** PCA finds linear combinations. For non-linear structure, use t-SNE or UMAP (Extension).
- **It's not invariant to feature scaling.** Always scale first.

### Quick Check — PCA

**Q1.** Your dataset has 10 features. The first two principal components together explain 75% of variance. Is it safe to drop the other 8 components?

*Sample answer:* "Safe" depends on the downstream use. For visualisation, yes — 2 components is what fits on a screen. For modelling, you're losing 25% of variance — that might or might not matter. Plot the cumulative variance and decide where to cut.

**Q2.** Why must you scale numeric features before PCA?

*Sample answer:* PCA maximises variance. Without scaling, the feature with the biggest natural range (e.g., `avg_monthly_spend` in £) will dominate PC1 — not because it's most informative, but because it has the most numeric variance. Scaling puts every feature on equal footing.

**Q3.** What does it mean if PC1 has near-equal positive loadings on all your numeric features?

*Sample answer:* PC1 has captured a generic "magnitude" or "size" dimension. Customers who score high on PC1 are large on every measurement — high spend, long tenure, many purchases. That's often the "engagement" or "activity level" axis.

---

## Part 2 — K-Means Clustering

### What it does

K-Means partitions your data into K clusters by minimising the total *within-cluster sum of squared distances*. The algorithm alternates:

1. Assign each point to its nearest cluster centre (centroid)
2. Move each centroid to the mean of the points assigned to it
3. Repeat until centroids stop moving

The result: K groups where points within a group are close to each other and far from other groups.

### Mechanics in sklearn

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=4, random_state=42, n_init=10)
labels = kmeans.fit_predict(X_scaled)
centroids = kmeans.cluster_centers_
```

Two important parameters:
- **`n_clusters` (K)** — must be set in advance. The big question (see below).
- **`n_init`** — number of restarts with different random initialisations. Default 10 is fine.

### How to choose K

K-Means doesn't tell you K. You have to decide. Three standard approaches:

**Elbow method.** Plot *inertia* (within-cluster sum of squared distances) against K. As K grows, inertia falls — but at some point the marginal gain shrinks. The "elbow" in the curve is a reasonable K.

```python
inertias = []
for k in range(2, 11):
    km = KMeans(n_clusters=k, random_state=42, n_init=10)
    km.fit(X_scaled)
    inertias.append(km.inertia_)
# plot inertias vs k → look for the elbow
```

**Silhouette score.** For each point, compute how close it is to its own cluster vs the nearest other cluster. Score ranges -1 (wrong cluster) to +1 (perfectly placed). Pick K with the highest mean silhouette.

```python
from sklearn.metrics import silhouette_score
for k in range(2, 11):
    km = KMeans(n_clusters=k, random_state=42, n_init=10)
    labels = km.fit_predict(X_scaled)
    score = silhouette_score(X_scaled, labels)
    # higher = better
```

**Business judgement.** K=4 might give a slightly lower silhouette than K=7 — but 4 segments are easier to act on than 7. Don't ignore the operational reality.

### Interpreting clusters

After fitting, for each cluster compute the mean of every feature. Compare to the global mean to spot what makes that cluster distinctive.

```python
df["cluster"] = labels
profile = df.groupby("cluster").mean()
# row = cluster id, column = feature, value = cluster's mean
```

Then write one sentence per cluster:
- "Cluster 0 has high spend AND high tenure AND low returns. → loyal high-value customers."
- "Cluster 1 has short tenure AND high returns. → new + dissatisfied; high churn risk."

### When K-Means struggles

- **Non-globular clusters.** K-Means assumes spherical clusters. For curved or stringy clusters, try DBSCAN (Extension).
- **Highly imbalanced cluster sizes.** K-Means tends to find equal-sized clusters. If reality has one huge cluster and a few tiny ones, K-Means may force-split the big cluster.
- **High-dimensional data.** Distances stop being meaningful in very high dimensions (the "curse of dimensionality"). Run PCA first to reduce to 2–10 components.

### Quick Check — K-Means

**Q1.** Your elbow plot looks like a smooth curve with no obvious bend. What do you do?

*Sample answer:* Use silhouette score AND business judgement. If silhouette also doesn't peak clearly, the data probably doesn't have natural clusters at the K-Means assumption (spherical). Try DBSCAN, or accept that your "clusters" are arbitrary cuts you're choosing for operational reasons.

**Q2.** K-Means gives you 4 clusters. Cluster 3 has only 12 customers; the others have ~2,500 each. What does this likely mean?

*Sample answer:* Cluster 3 is a tiny outlier group — probably a handful of customers very different from everyone else. You found an anomaly cluster. Two options: (a) report cluster 3 as a "watch list" and use the other three for segmentation, or (b) drop those 12 customers as outliers and re-run K-Means.

**Q3.** Should you scale features before K-Means?

*Sample answer:* Yes — K-Means is distance-based. Features with bigger ranges dominate. Use `StandardScaler` before fitting.

---

## Part 3 — Isolation Forest (Anomaly Detection)

### What it does

An anomaly is a point that's *unlike everything else*. Isolation Forest builds an ensemble of random trees that recursively split the data. Anomalies tend to get *isolated* (land in a leaf alone) with very few splits, while normal points need many splits to be isolated.

The output: an *anomaly score* per point. Lower scores mean "more anomalous."

### Mechanics in sklearn

```python
from sklearn.ensemble import IsolationForest

iso = IsolationForest(
    n_estimators=100,         # number of trees
    contamination=0.05,       # expected fraction of anomalies
    random_state=42,
)
# Returns -1 for anomalies, +1 for normal
predictions = iso.fit_predict(X_scaled)
scores = iso.score_samples(X_scaled)   # raw scores (lower = more anomalous)
```

### The `contamination` parameter

This is your prior belief about the fraction of anomalies in the data. If you don't know:
- Set `contamination='auto'` and let sklearn pick a threshold
- Set a small explicit value (0.01–0.05) for "very strict"
- Set a larger value (0.10) for "more permissive"

**`contamination` controls only the threshold for the binary label.** The continuous `score_samples()` output doesn't depend on it. You can re-threshold after fitting if you change your mind.

### When to use Isolation Forest

- **Fraud detection.** Card transactions that look unlike a user's normal pattern.
- **Manufacturing defects.** Products that don't match the typical sensor readings.
- **Network security.** Connections that don't match normal traffic patterns.
- **Data quality.** Spotting corrupted rows before they affect downstream models.

### Compared to other anomaly methods

| Method | Strengths | Weaknesses |
|---|---|---|
| **Isolation Forest** | Fast, works in high dimensions, no distance metric needed | Sensitive to parameter choices |
| **Z-score / IQR** | Simple, interpretable | Univariate; misses multivariate anomalies |
| **One-Class SVM** | Strong theoretical foundation | Slow on large datasets |
| **Autoencoders** | Captures complex patterns | Need lots of data + training time |

Isolation Forest is the default first try in industry.

### Interpreting an anomaly

For the most anomalous points, look at every feature and ask: *why does this point look weird?*

```python
# Get the most anomalous customer
most_anomalous_idx = scores.argmin()
print(X.iloc[most_anomalous_idx])
# compare to median
print("\nDataset median:")
print(X.median())
```

The features that diverge most from the median are the "reasons" the model flagged that point.

### Quick Check — Isolation Forest

**Q1.** You set `contamination=0.05` on 10,000 customers. About how many will be flagged as anomalies?

*Sample answer:* About 500 (5% of 10,000). The model picks a threshold on `score_samples` so that ~5% of training rows score below it.

**Q2.** An anomaly is flagged. Its `last_login_days_ago` is missing, its `tenure_months` is 70 (long), and its `support_tickets_quarter` is 8 (high). Is it really an anomaly?

*Sample answer:* Probably yes — long-tenured customers who suddenly stop logging in but file lots of support tickets are unusual. Whether they're fraud or just unhappy is a question for the support team. The model surfaces; humans investigate.

**Q3.** Can you use Isolation Forest on new data after fitting?

*Sample answer:* Yes. Use `iso.predict(new_X)` or `iso.score_samples(new_X)`. The model is fit once on training data; you can score arbitrary new rows.

---

## Friday — what Sarah presents

By Friday afternoon Sarah has three deliverables:

1. **A 2D PCA plot** of the customer base — visible structure, even though there are 10+ features
2. **Four customer segments** named and profiled — "Loyal Premium," "New & Wary," "Steady Mid-Tier," and "Bargain Hunters"
3. **A list of ~250 anomalous customers** — for the customer success team to investigate

Marcus listens, then asks:

> *"OK. Now — our sales are seasonal, and we want to forecast next quarter's revenue. Can you do that?"*

That question — **predicting time-ordered data** — is the engine of **L06 (Time Series Forecasting).**

---

## Glossary

See [`reference.md`](./reference.md) for a 20-term glossary covering dimensionality reduction, clustering, and anomaly-detection language used in this lesson.
