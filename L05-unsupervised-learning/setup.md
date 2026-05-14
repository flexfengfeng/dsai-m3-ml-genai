# Setup — L05

Skip if you already have the `dsai-m3` environment from L01–L04. All required packages (`scikit-learn` for PCA, KMeans, and IsolationForest) are already installed.

---

## Create the environment

From the **root of the learner-edition folder**:

```bash
conda env create -f L05-unsupervised-learning/environment.yml
```

This takes 10–15 minutes the first time.

---

## Activate

```bash
conda activate dsai-m3
```

Verify by running:

```bash
python -c "from sklearn.decomposition import PCA; from sklearn.cluster import KMeans; from sklearn.ensemble import IsolationForest; print('sklearn ready for L05')"
```

You should see:
```
sklearn ready for L05
```

---

## (Optional) UMAP

The Core L05 content uses PCA + K-Means + Isolation Forest from sklearn. If you want to try **UMAP** (a non-linear alternative to PCA, popular for visualisation) in the Extension section:

```bash
pip install umap-learn
```

---

## Launch the notebooks

From inside `L05-unsupervised-learning/`:

```bash
jupyter notebook
```

Open `notebooks/01_monday_morning.ipynb` to begin.

---

## Troubleshooting

- **`FileNotFoundError: northstar_customers.csv`** — run notebooks from inside `L05-unsupervised-learning/notebooks/`.
- **Plots look small** — add `plt.rcParams["figure.figsize"] = (12, 5)` near the top of your notebook. Most are already configured.
- **`ImportError: No module named umap`** — only needed for the Extension content. Run `pip install umap-learn`.
