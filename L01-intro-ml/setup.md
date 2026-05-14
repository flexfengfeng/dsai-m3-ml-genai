# Setup Guide — DSAI M3 (All Lessons)

**Do this once before you start Lesson 1.** The same environment works for all 10 lessons.

Estimated time: 10–15 minutes (plus ~5 minutes for the first model download).

---

## What you will install

A Python 3.11 environment with the libraries used across the course — pandas and scikit-learn for the classical ML lessons, plus PyTorch and Hugging Face Transformers for the deep-learning and GenAI lessons.

You do **not** need to understand these libraries yet. The notebooks will introduce them as needed.

---

## Option A — Conda (recommended if you have Anaconda or Miniconda)

1. Open a terminal (macOS/Linux) or Anaconda Prompt (Windows).

2. Navigate to this folder:
   ```bash
   cd path/to/L01-intro-ml
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

6. Your browser should open. Navigate to `notebooks/02_what_is_ml.ipynb` to verify everything works. The first code cell should print `✅ Libraries loaded — you're ready to go!`.

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
   pip install numpy pandas matplotlib seaborn scikit-learn jupyter ipywidgets textblob
   pip install transformers torch datasets
   ```

4. Launch Jupyter:
   ```bash
   jupyter notebook
   ```

## Option C — Google Colab (zero install)

If installing locally is painful, use [Google Colab](https://colab.research.google.com). Upload the notebook you want to run; the libraries used in this course are pre-installed. You will only need to `!pip install transformers` inside the notebook for the GenAI lessons.

**Colab tradeoff:** you cannot save your environment between sessions the same way, and free GPU access is time-limited. Fine for learning, less fine if you want to keep iterating on one dataset across several days.

---

## Verify your setup

Open a terminal in your activated environment and run:

```bash
python -c "import numpy, pandas, sklearn, matplotlib; print('classical ML OK')"
python -c "import torch, transformers; print('deep learning OK')"
```

Both should print the success message. If you see an `ImportError`, re-run the install step for that library.

---

## Troubleshooting

**"conda: command not found"**
You don't have Conda installed. Either install [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or use Option B (pip).

**First `transformers` model download is slow**
The first time a notebook calls `pipeline("sentiment-analysis")`, it downloads a ~250 MB model. Expect 1–5 minutes depending on your connection. Subsequent runs use the cached model.

**Jupyter launches but notebooks show a different kernel**
In the notebook, go to `Kernel → Change kernel` and pick `Python (dsai-m3)`. If it is not listed, run:
```bash
python -m ipykernel install --user --name dsai-m3 --display-name "Python (dsai-m3)"
```

**I am behind a corporate proxy / VPN**
Model downloads may fail. Work around either by downloading the model on a home network and copying the `~/.cache/huggingface` folder, or by using Colab (Option C).

---

## What to do next

Return to [README.md](./README.md) and start with **Phase 1 — Pre-Class Self-Study** (`pre-class.md`).
