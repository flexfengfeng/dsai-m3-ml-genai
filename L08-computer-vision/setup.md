# L08 — Computer Vision — Setup

Environment setup is at the repo root: ➡ **[../SETUP.md](../SETUP.md)**

If you completed setup for an earlier lesson, your `dsai-m3` environment already has everything L08 needs. Skip ahead.

---

## What's new this lesson

**Dependencies:** `torch >= 2.2`, `torchvision >= 0.17` (install once per `SETUP.md`).

**Data:** Fashion-MNIST (~30 MB) and CIFAR-10 (~170 MB) **auto-download** on first run via `torchvision.datasets.FashionMNIST(download=True)` / `CIFAR10(download=True)`. Both datasets are gitignored so they never bloat the repo. First-run download takes 1-3 min.

**Models downloaded on first run:** Transfer-learning notebooks download `resnet18` ImageNet weights (~45 MB) and optionally `mobilenet_v3_small` (~10 MB). Cached in `~/.cache/torch/hub/`.


## Use Colab for GPU work

**Use Colab if you don't have a GPU.** The CIFAR-10 + ResNet18 fine-tune in `03_first_cnn.ipynb`, `04_transfer_learning.ipynb`, and `assignment.ipynb` is ~30 min on CPU vs ~2 min on a T4 GPU. Each of those notebooks has an **Open in Colab** badge at the top.


## Sanity check

Open any notebook in this lesson, pick the `dsai-m3` kernel, run the setup cell. If anything errors, see **Troubleshooting** in [../SETUP.md](../SETUP.md).
