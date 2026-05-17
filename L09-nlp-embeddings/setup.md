# L09 — NLP & Embeddings — Setup

Environment setup is at the repo root: ➡ **[../SETUP.md](../SETUP.md)**

If you completed setup for an earlier lesson, your `dsai-m3` environment already has everything L09 needs. Skip ahead.

---

## What's new this lesson

**Dependencies:** `sentence-transformers >= 3.0` (install once per `SETUP.md`).

**Data:** `notebooks/data/northstar_catalogue.csv` (76 products, ~15 KB) + `notebooks/data/recipes.csv` (30 recipes, ~6 KB).

**Models downloaded on first run:** First run downloads `all-MiniLM-L6-v2` (~80 MB) from Hugging Face into `~/.cache/huggingface/`. `optional_extensions.ipynb` additionally downloads `paraphrase-multilingual-MiniLM-L12-v2` (~280 MB).


## Sanity check

Open any notebook in this lesson, pick the `dsai-m3` kernel, run the setup cell. If anything errors, see **Troubleshooting** in [../SETUP.md](../SETUP.md).
