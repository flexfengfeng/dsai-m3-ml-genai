# Setup — L07

L07 introduces **PyTorch** — a new dependency.

---

## Install PyTorch

Inside your `dsai-m3` conda env:

```bash
pip install torch
```

(For GPU support, follow the official [PyTorch install page](https://pytorch.org/get-started/locally/). For this lesson CPU-only is fine — the model is small.)

---

## Verify

```bash
python -c "import torch; print(f'PyTorch {torch.__version__} ready')"
```

You should see something like:
```
PyTorch 2.x ready
```

---

## Launch the notebooks

From inside `L07-neural-networks/`:

```bash
jupyter notebook
```

Open `notebooks/01_monday_morning.ipynb`.

---

## Troubleshooting

- **`ImportError: No module named torch`** — run `pip install torch` inside the active conda env.
- **`RuntimeError: CUDA error: no kernel image is available for execution on the device`** — happens when PyTorch picks GPU by accident. Force CPU with `device = torch.device("cpu")`.
- **Training is slow** — the L07 MLP has thousands of parameters, not millions. Each epoch should take seconds on CPU. If it's much slower, check you're using `DataLoader(batch_size=64)` not iterating row-by-row.
