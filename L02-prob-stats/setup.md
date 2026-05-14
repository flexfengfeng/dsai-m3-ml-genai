# Setup Guide — DSAI M3 (All Lessons)

**Do this once before you start any lesson.** The same environment works for all 10 lessons. If you have already followed this guide for L01, jump to the "Already set up from L01?" callout below.

Estimated time: 10–15 minutes (plus ~5 minutes for the first model download).

> **Already set up from L01?** Your `dsai-m3` environment already includes everything you need for L02 (scipy and statsmodels were added). Just activate it and launch Jupyter. If you get an `ImportError` for `scipy`, run `conda install scipy` or `pip install scipy` inside the activated environment.

---

## What you will install

A Python 3.11 environment with the libraries used across the course — pandas, scipy, and statsmodels for the statistics lessons, plus PyTorch and Hugging Face Transformers for the deep-learning and GenAI lessons.

You do **not** need to understand these libraries yet. The notebooks will introduce them as needed.

---

## Option A — Conda (recommended if you have Anaconda or Miniconda)

1. Open a terminal (macOS/Linux) or Anaconda Prompt (Windows).

2. Navigate to this folder:
   ```bash
   cd path/to/L02-prob-stats
   ```

3. Create the environment from `environment.yml`:
   ```bash
   conda env create -f environment.yml
   ```

4. Activate it:
   ```bash
   conda activate dsai-m3
   ```

5. Launch Jupyter:
   ```bash
   jupyter notebook
   ```

6. Your browser should open. Navigate to `notebooks/01_monday_morning.ipynb` to verify everything works. The second code cell should print `✅ Libraries loaded — you're ready to go!`.

## Option B — pip (if you don't use Conda)

1. Open a terminal. Make sure you have Python 3.11 installed (`python --version`).

2. Create a virtual environment:
   ```bash
   python -m venv dsai-m3-env
   source dsai-m3-env/bin/activate   # macOS / Linux
   dsai-m3-env\Scripts\activate      # Windows
   ```

3. Install the libraries:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn scipy statsmodels jupyter ipywidgets textblob
   pip install transformers torch datasets
   ```

4. Launch Jupyter:
   ```bash
   jupyter notebook
   ```

## Option C — Google Colab (zero install)

If installing locally is painful, use [Google Colab](https://colab.research.google.com). Upload the notebook you want to run. You will need to add this cell at the top of each notebook:

```python
!pip install scipy statsmodels
```

Most other libraries (numpy, pandas, matplotlib, seaborn) are pre-installed on Colab.

**Colab tradeoff:** you cannot save your environment between sessions the same way, and free GPU access is time-limited. Fine for learning, less fine if you want to keep iterating on one dataset across several days.

---

## Verify your setup

Open a terminal in your activated environment and run:

```bash
python -c "import numpy, pandas, sklearn, matplotlib, scipy, statsmodels; print('L02 stats OK')"
```

Should print `L02 stats OK`. If you see an `ImportError`, re-run the install step for that library.

---

## Troubleshooting

**"conda: command not found"**
You don't have Conda installed. Either install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or use Option B (pip).

**"No module named scipy"**
Run `pip install scipy --break-system-packages` or `conda install scipy` in your activated environment.

**Jupyter launches but notebooks show a different kernel**
In the notebook, go to `Kernel → Change kernel` and pick `Python (dsai-m3)`. If it is not listed, run:
```bash
python -m ipykernel install --user --name dsai-m3 --display-name "Python (dsai-m3)"
```

---

## What to do next

Return to [README.md](./README.md) and start with **Phase 1 — Pre-Class Self-Study** (`pre-class.md`).
