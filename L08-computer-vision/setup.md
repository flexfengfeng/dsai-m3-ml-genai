# L08 Setup

## Prerequisites

You should already have the `dsai-m3` conda environment from L01. If not, see L01's setup.md first.

## New dependencies for L08

L07 added `torch`. L08 adds `torchvision`:

```bash
conda activate dsai-m3
pip install torch torchvision
```

Versions known to work:
- `torch >= 2.2`
- `torchvision >= 0.17`

## Dataset (Fashion-MNIST)

The notebooks load Fashion-MNIST via `torchvision.datasets.FashionMNIST`. The first time you run notebook 01 it will download ~30 MB into `notebooks/data/fmnist/`. The data files are already shipped with this repo, so you should not see a download progress bar.

For the assignment, the CIFAR-10 dataset (~170 MB) downloads on first use into `notebooks/data/cifar10/`.

## macOS note (PyTorch threading)

Same as L07 — if a kernel crashes on `model.fit`-style cells, restart Jupyter with:

```bash
OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 jupyter notebook
```

We also call `torch.set_num_threads(1)` near the top of each PyTorch notebook for stability.

## GPU (optional)

If you have a CUDA GPU or Apple Silicon Mac, the notebooks will auto-detect and use it:

```python
device = "cuda" if torch.cuda.is_available() else ("mps" if torch.backends.mps.is_available() else "cpu")
```

All notebooks complete in under 5 minutes on CPU, so a GPU is **not** required.

## Sanity check

After install, run:

```bash
python -c "import torch, torchvision; print(torch.__version__, torchvision.__version__)"
```

You should see something like `2.12.0 0.27.0`.
