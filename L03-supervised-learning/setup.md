# Setup — L03

You only need to do this once. If you already set up the `dsai-m3` environment for L01 or L02, you can skip directly to the [Activate the environment](#activate-the-environment) section.

---

## Install a Python environment manager

If you do not already have `conda` or `mamba` installed, install Miniconda first:
[https://docs.conda.io/projects/miniconda/en/latest/](https://docs.conda.io/projects/miniconda/en/latest/)

---

## Create the `dsai-m3` environment

From the **root of the learner-edition folder** (the one containing `L01-intro-ml`, `L02-prob-stats`, etc.), run:

```bash
conda env create -f L03-supervised-learning/environment.yml
```

This takes 10–15 minutes the first time. The same environment works for all 10 lessons in this module.

---

## Activate the environment

```bash
conda activate dsai-m3
```

Verify by running:

```bash
python -c "import pandas, sklearn; print('pandas', pandas.__version__, '· sklearn', sklearn.__version__)"
```

You should see something like:
```
pandas 2.x.x · sklearn 1.5.x
```

---

## Launch the notebooks

From inside `L03-supervised-learning/`:

```bash
jupyter notebook
```

Open `notebooks/01_monday_morning.ipynb` to begin.

---

## Troubleshooting

- **`ImportError: No module named sklearn`** — your environment didn't activate. Run `conda activate dsai-m3` and try again.
- **`FileNotFoundError: northstar_churn.csv`** — make sure you're running notebooks from inside `L03-supervised-learning/notebooks/` so the relative path `data/northstar_churn.csv` resolves.
- **Plots not showing** — add `%matplotlib inline` near the top of the notebook (most are already configured for this).
