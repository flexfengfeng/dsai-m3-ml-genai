# L09 Setup

## Prerequisites

You should have the `dsai-m3` conda environment from L01-L08. If not, see L01's setup.md.

## New dependencies

L09 adds:

```bash
conda activate dsai-m3
pip install sentence-transformers
```

This pulls in `transformers`, `tokenizers`, `huggingface-hub`, and `safetensors` as dependencies. Total disk: ~600 MB including the pretrained model weights downloaded on first use.

Versions known to work:
- `sentence-transformers >= 3.0`
- `transformers >= 4.40`

## Pretrained model

The notebooks load **`all-MiniLM-L6-v2`** from Hugging Face — a small, fast sentence-transformer model (22M parameters, 384-dim embeddings, ~80 MB). The first time you run notebook 03 it downloads to `~/.cache/huggingface/hub/`.

If your environment has no internet, pre-download in advance:

```bash
python -c "from sentence_transformers import SentenceTransformer; SentenceTransformer('all-MiniLM-L6-v2')"
```

## macOS note

Same PyTorch threading caveat as L07-L08. If a kernel crashes during embedding computation, restart Jupyter with:

```bash
OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 jupyter notebook
```

## Sanity check

After install, run:

```bash
python -c "from sentence_transformers import SentenceTransformer; print(SentenceTransformer('all-MiniLM-L6-v2').encode('hello').shape)"
```

You should see `(384,)` — the embedding dimension of this model.
