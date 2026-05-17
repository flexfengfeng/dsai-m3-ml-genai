# L01 Setup

L01 is the first lesson, so it's also the first time you'll run the environment. The full setup walkthrough (Mac, Windows WSL, VS Code, Colab) lives at the **repo root**:

➡ **[../SETUP.md](../SETUP.md)** — start there.

Once that's done, come back to this page for L01-specific notes.

---

## L01-specific dependencies

L01 uses pretrained sentiment / NLP models via Hugging Face's `transformers` library. These are part of the cross-lesson extras you install once in `SETUP.md`:

```bash
pip install transformers torch
```

No other L01-specific packages.

## L01 dataset

There is no dataset to download — L01 uses 5-25 hand-crafted product reviews defined directly in the notebooks.

## L01 model download

The first time you run `02_what_is_ml.ipynb`, it downloads `distilbert-base-uncased-finetuned-sst-2-english` (~268 MB) from Hugging Face into `~/.cache/huggingface/`. This takes 30 sec to 2 min depending on your connection. After that it's cached locally and the load is instant.

## Sanity check

After completing the root SETUP.md, open `notebooks/02_what_is_ml.ipynb`, select the `dsai-m3` kernel, and run the first code cell. You should see:

```
✅ Libraries loaded — you're ready to go!
```

Then run cell 9 (the pipeline load) — first run downloads the model. Then run the rest. The whole notebook should complete in under 3 minutes (including the first-run download).

If anything hangs or errors, see the **Troubleshooting** section in [../SETUP.md](../SETUP.md). The most common L01 issue is the macOS OpenMP hang on the sentiment loop — the repo's `.env` file already caps thread counts, but it only works if VS Code is rooted at the repo top level.
