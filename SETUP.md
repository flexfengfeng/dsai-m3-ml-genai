# Environment Setup Guide

This guide gets you from zero to running L01-L10 on your machine in about 15 minutes.

**Supported environments:**
- **macOS** (Apple Silicon — M1 or later)
- **Windows 10/11 with WSL2** (Ubuntu 22.04 recommended)
- **Google Colab** (for L08 and L10 if you don't have a GPU)

**Supported IDE / runtime:**
- **VS Code + Jupyter extension** (recommended for L01-L10)
- **Google Colab** (for GPU-heavy lessons: L08 transfer learning, L10 LLM generation)

You do **not** need Jupyter Notebook or JupyterLab installed separately — VS Code handles everything.

---

## TL;DR — fastest path

```bash
# 1. Clone the repo
git clone https://github.com/flexfengfeng/dsai-m3-ml-genai.git
cd dsai-m3-ml-genai

# 2. Create the conda environment
conda env create -f L01-intro-ml/environment.yml
conda activate dsai-m3

# 3. Install the cross-lesson extras
pip install torch torchvision sentence-transformers transformers

# 4. Launch VS Code in the repo root
code .
```

Then in VS Code:
1. Open any notebook (e.g., `L01-intro-ml/notebooks/02_what_is_ml.ipynb`)
2. Top-right → **Select Kernel** → `dsai-m3` (Python 3.11)
3. **Run All**

If a notebook hangs or errors, scroll down to **Troubleshooting**.

---

## 1. Install Python via Miniconda

We use conda to manage a self-contained Python environment for the course. This avoids polluting your system Python.

### macOS (Apple Silicon)

```bash
curl -L https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh -o miniconda.sh
bash miniconda.sh -b -p $HOME/miniconda3
$HOME/miniconda3/bin/conda init "$(basename $SHELL)"

# Restart your terminal, then verify
conda --version
```

### Windows WSL (Ubuntu)

Open Ubuntu in WSL, then:

```bash
curl -L https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh
bash miniconda.sh -b -p $HOME/miniconda3
$HOME/miniconda3/bin/conda init bash

# Restart your terminal, then verify
conda --version
```

> **Windows users — do NOT install Miniconda on Windows directly.** Use WSL. Notebooks that use PyTorch are much more reliable on Linux (WSL is Linux) than native Windows.

---

## 2. Clone the repo

### macOS

```bash
mkdir -p ~/repos
cd ~/repos
git clone https://github.com/flexfengfeng/dsai-m3-ml-genai.git
cd dsai-m3-ml-genai
```

### Windows WSL

Clone **inside WSL**, not in Windows file system. Notebooks run *much* faster on the WSL filesystem (`~/repos/...`) than on `/mnt/c/...`.

```bash
mkdir -p ~/repos
cd ~/repos
git clone https://github.com/flexfengfeng/dsai-m3-ml-genai.git
cd dsai-m3-ml-genai
```

---

## 3. Create the conda environment

From the cloned repo root:

```bash
conda env create -f L01-intro-ml/environment.yml
conda activate dsai-m3
```

This creates an environment named `dsai-m3` with Python 3.11 and the L01 baseline packages. **Reuse the same environment for every lesson** — later lessons just `pip install` the new deps they need.

### Install cross-lesson extras (L07-L10)

Some lessons (L07 neural networks onward) add a few packages. Install them all at once now to avoid surprises later:

```bash
pip install torch torchvision sentence-transformers transformers
```

Versions known to work:
- `torch >= 2.2`
- `torchvision >= 0.17`
- `sentence-transformers >= 3.0`
- `transformers >= 4.40`

---

## 4. Install VS Code + Jupyter extension

### macOS

Download from https://code.visualstudio.com/Download. Drag to Applications.

### Windows WSL

Install VS Code **on Windows** (not in WSL). VS Code itself runs on Windows; it connects to WSL through the Remote-WSL extension. Download from https://code.visualstudio.com/Download.

### Required extensions (both platforms)

In VS Code, **Cmd/Ctrl+Shift+X** opens the Extensions sidebar. Install:

1. **Python** (Microsoft) — required
2. **Jupyter** (Microsoft) — required
3. **WSL** (Microsoft) — **only for Windows WSL users**; lets you open the WSL filesystem from VS Code

---

## 5. Open the repo in VS Code

### macOS

```bash
cd ~/repos/dsai-m3-ml-genai
code .
```

### Windows WSL

```bash
cd ~/repos/dsai-m3-ml-genai
code .
```

This launches VS Code with WSL on the backend. You'll see a green `WSL: Ubuntu` indicator in the bottom-left. The repo's `.vscode/settings.json` (already committed) will configure the environment file automatically.

---

## 6. Verify your setup

Open `L01-intro-ml/notebooks/02_what_is_ml.ipynb`.

1. **Top-right of the notebook → "Select Kernel"** → pick `dsai-m3` (Python 3.11)
2. Run the **first code cell** (`import pandas as pd...`)
3. You should see `✅ Libraries loaded — you're ready to go!`

If that prints, your environment is ready. Run the rest of the notebook to confirm.

---

## When to use Google Colab instead

Two lessons benefit significantly from a GPU:

| Lesson | Why a GPU helps |
|---|---|
| **L08 — Computer Vision** | CIFAR-10 + ResNet18 fine-tuning is ~30 min on CPU vs ~2 min on T4 GPU |
| **L10 — Transformers & GenAI** | SmolLM2 generation is ~10s/response on CPU vs <1s on GPU |

For these two lessons specifically, you can run the notebooks in **Google Colab** (free tier includes a T4 GPU).

Each GPU-friendly notebook has an **"Open in Colab"** badge at the top — click it to launch in Colab. The notebook has a setup cell that:
1. Clones this repo into Colab's file system
2. Installs the missing dependencies
3. Switches to the right working directory

After that the notebook runs identically to the local version.

**Colab caveats:**
- Free-tier sessions expire after ~12 hours; save your work
- File system is ephemeral — anything you write is lost when the session ends
- GPU runtime must be enabled: **Runtime → Change runtime type → T4 GPU**

For all other lessons (L01-L07, L09), CPU is fine and the local setup is faster.

---

## Troubleshooting

### "The notebook hangs forever when I run the sentiment model" (L01 NB02)

This is the OpenMP scheduler issue on macOS — PyTorch threads can lock up the CPU. The repo's `.env` file caps the thread count, but it only works if VS Code picked it up.

**Verify the env var loaded** — in any code cell:

```python
import os
print(os.environ.get("OMP_NUM_THREADS"))   # should print "1"
```

If it prints `None`:
1. Confirm VS Code's workspace root is the repo root (left sidebar shows `L01-intro-ml/`, `L02-prob-stats/`, etc., and `.env`)
2. **Cmd/Ctrl+Shift+P → "Developer: Reload Window"**
3. Try again

If VS Code's kernel itself is stuck and **Restart Kernel** hangs:

```bash
# In a fresh terminal
pkill -9 -f ipykernel
pkill -9 -f vscode-jupyter
```

Then reload VS Code (Cmd+Q + relaunch).

### "I get a `ModuleNotFoundError` for torch / transformers / sentence-transformers"

You didn't run step 3 (install cross-lesson extras). Run it now:

```bash
conda activate dsai-m3
pip install torch torchvision sentence-transformers transformers
```

### "Kernel `dsai-m3` doesn't appear in the kernel picker"

VS Code's Python extension caches kernels. Reload VS Code:
**Cmd/Ctrl+Shift+P → "Developer: Reload Window"**

If still missing:

```bash
conda activate dsai-m3
python -m ipykernel install --user --name dsai-m3 --display-name "Python (dsai-m3)"
```

Then reload VS Code again.

### "WSL is slow"

You're probably running notebooks from `/mnt/c/...` (the Windows filesystem). Move them to `~/repos/...` inside WSL — notebook execution gets 5-10× faster.

### Something else broke

Open an issue on the repo: https://github.com/flexfengfeng/dsai-m3-ml-genai/issues. Include:
- macOS or WSL? (and version)
- Output of `conda --version`, `python --version`, `pip show torch transformers`
- The full error traceback
- Which notebook + which cell

---

## Quick reference

| Task | Command |
|---|---|
| Activate the env | `conda activate dsai-m3` |
| Update deps | `pip install -U torch torchvision sentence-transformers transformers` |
| Launch VS Code in repo | `code .` (from the repo root) |
| Force-kill stuck kernel | `pkill -9 -f ipykernel` |
| Reload VS Code | `Cmd/Ctrl+Shift+P → "Developer: Reload Window"` |
| Open a notebook in Colab | Click the **"Open in Colab"** badge at the top of the notebook |
