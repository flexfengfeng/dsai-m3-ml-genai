# L10 — Transformers & GenAI — Setup

Environment setup is at the repo root: ➡ **[../SETUP.md](../SETUP.md)**

If you completed setup for an earlier lesson, your `dsai-m3` environment already has everything L10 needs. Skip ahead.

---

## What's new this lesson

**Dependencies:** `transformers >= 4.40`, `sentence-transformers >= 3.0` (install once per `SETUP.md`).

**Data:** Reuses `northstar_catalogue.csv` from L09 (copied into `L10-transformers-genai/notebooks/data/`).

**Models downloaded on first run:** First run downloads three models from Hugging Face: `distilbert-sst-2` (~268 MB), `bert-base-NER` (~436 MB), `SmolLM2-360M-Instruct` (~720 MB). `optional_extensions.ipynb` adds `bart-large-mnli` (~1.6 GB) and `paraphrase-multilingual-MiniLM-L12-v2` (~280 MB). Total cache after running the whole lesson: ~3 GB in `~/.cache/huggingface/`.


## Use Colab for GPU work

**Use Colab if you don't have a GPU.** SmolLM2 generation in `03_using_an_llm.ipynb`, `04_rag_pipeline.ipynb`, and `assignment.ipynb` is ~5-15 tokens/sec on CPU vs near-instant on a T4 GPU. Each of those notebooks has an **Open in Colab** badge at the top.


## `.env` file — required for macOS

The `dsai-m3` conda environment on macOS links two copies of the OpenMP runtime (`libomp.dylib`), which causes a **kernel crash on `import torch`** unless you tell it to continue safely.

This is handled by a `.env` file at the repo root (`learner-edition/.env`). VS Code reads it automatically via `.vscode/settings.json`. **Check the file exists and contains these four lines:**

```
OMP_NUM_THREADS=1
MKL_NUM_THREADS=1
TOKENIZERS_PARALLELISM=false
KMP_DUPLICATE_LIB_OK=TRUE
```

If the file is missing, create it at `learner-edition/.env` with the contents above.

> **After adding or editing `.env`, restart the VS Code Jupyter kernel** — VS Code only reads the file at kernel startup, not mid-session. Use the **Restart** button in the notebook toolbar (or the command palette: *Jupyter: Restart Kernel*).


## Sanity check

Open any notebook in this lesson, pick the `dsai-m3` kernel, run the setup cell. If anything errors, see **Troubleshooting** in [../SETUP.md](../SETUP.md).
