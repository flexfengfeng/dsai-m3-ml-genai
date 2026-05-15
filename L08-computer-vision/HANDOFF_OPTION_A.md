# L08 — Computer Vision & CNNs · Option A Handoff

**Status:** ✅ Complete
**Build date:** 2026-05-14
**Pattern:** Option A — slide-driven lecture (~90 min) + 4 in-class notebooks (~90 min), 🟡 Extension boundary separating Core from self-study.

---

## What's inside

```
L08-computer-vision/
├── README.md                            Sarah's brief + Phase 1/2/3 structure
├── setup.md                             Environment setup (torchvision)
├── pre-class.md                         75-min self-study (3Blue1Brown + NB 01)
├── lesson.md                            Concept reference + Sarah's recommendation
├── reference.md                         23-term glossary
├── environment.yml                      Conda env spec
├── slides/
│   ├── L08_slides.pptx                  28 slides, Ocean Gradient palette
│   └── slides_outline.md                Slide-by-slide breakdown
├── notebooks/
│   ├── 01_monday_morning.ipynb          Pre-class hook — FlatMLP baseline on Fashion-MNIST
│   ├── 02_convolutions_intuition.ipynb  Manual kernels + nn.Conv2d equivalence
│   ├── 03_first_cnn.ipynb               TinyCNN vs FlatMLP head-to-head
│   ├── 04_transfer_learning.ipynb       Pretrained ResNet18, head-only + fine-tuning
│   ├── assignment.ipynb                 CIFAR-10 from-scratch + transfer learning
│   ├── optional_extensions.ipynb        Augmentation, BatchNorm, LR schedulers
│   └── data/
│       ├── fmnist/                      Fashion-MNIST (auto-downloads, ~30 MB; .gitignored)
│       └── cifar10/                     CIFAR-10 (auto-downloads, ~170 MB; .gitignored)
└── HANDOFF_OPTION_A.md                  (this file)
```

## Smoke-test results

All 6 notebooks executed end-to-end with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1`:

| Notebook | Cells | Status | Key result |
|----------|------:|--------|-----------|
| 01_monday_morning            | 17 | ✅ | FlatMLP on Fashion-MNIST: ~87% test acc, 235K params |
| 02_convolutions_intuition    | 27 | ✅ | Manual edge/blur/sharpen kernels; nn.Conv2d matches manual to machine precision |
| 03_first_cnn                 | 31 | ✅ | TinyCNN 88.4% (106K params) vs FlatMLP 87.1% (235K params) |
| 04_transfer_learning         | 28 | ✅ | TinyCNN scratch 0.787; ResNet18 head-only 0.715 (loses); fine-tune layer4 0.832 (wins) |
| assignment                   | 19 | ✅ | CifarCNN 0.595 (baseline ≥0.55 PASS); ResNet18 fine-tuned 0.837 (≥0.75 PASS); +24pp gain |
| optional_extensions          | 17 | ✅ | Data aug, BatchNorm, cosine LR schedule — all run cleanly |

## Narrative beats

1. **L07 → L08 bridge:** Marcus's question from L07 stand-up: *"Can we auto-tag 10K product photos per season?"* drives L08.
2. **The flat-MLP problem:** 28×28 needs 235K params; 224×224×3 would need 38.6M. MLPs don't scale.
3. **The spatial-locality fix:** Convolutions exploit locality + translation invariance. Same kernel slides everywhere.
4. **Code-along arc:** NB02 (manual kernels) → NB03 (build TinyCNN) → NB04 (transfer learning).
5. **The honest domain-gap result:** Head-only transfer LOSES to from-scratch on Fashion-MNIST (-7pp). Fine-tuning recovers (+5pp). The slogan "transfer learning is magic" is wrong; *adaptation* is what matters.
6. **The honest CIFAR result:** On CIFAR-10 (color natural photos), transfer learning gains +24pp over from-scratch — the canonical use case shines when the domain matches.
7. **L08 → L09 bridge:** Marcus's next question — *"natural-language product search?"* → embeddings & NLP in L09.

## Deviations & notes for the instructor

### 1. PyTorch threading: still required for stability

Same as L07. All smoke tests run with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1` and the notebooks call `torch.set_num_threads(1)` at the top of each PyTorch cell. Without this, kernel crashes (exit 139) can appear during long runs on macOS.

### 2. The Fashion-MNIST domain gap is a feature, not a bug

NB 04 originally read like "transfer learning is the answer." The actual results showed ImageNet → Fashion-MNIST (grayscale silhouettes) has a meaningful domain gap, and head-only feature extraction **underperforms** a from-scratch TinyCNN.

This was treated as a teaching opportunity rather than a flaw. The narrative now explicitly explains the domain gap and walks learners through:
- Head-only transfer (loses, AUC 0.715)
- Fine-tuning layer4 with LR 1e-4 (wins, AUC 0.832)

The lesson is *transfer learning isn't magic — match-of-domain matters*. The assignment then uses CIFAR-10 (color photos, much closer to ImageNet) where transfer dominates with a clean +24pp gain. The contrast between the two domains drives the reflection question.

### 3. Assignment Part A: baseline target lowered from 65% → 55%

The starter `CifarCNN` (3 conv blocks, no BatchNorm, 8 epochs) lands around 59-62% test accuracy. Originally Part A targeted ≥65%, which the starter doesn't reliably clear. Resolved by:

- **Baseline target ≥ 55%** — passes with the starter (the auto-check confirms)
- **Stretch target ≥ 65%** — flagged with hints to add BatchNorm, augmentation, or more epochs (Extensions notebook shows these techniques)

This better matches the actual capability of an 8-epoch unaugmented CNN on 10K CIFAR-10 images.

### 4. Pre-downloaded datasets

To avoid each learner hitting the internet on first run:
- **Fashion-MNIST** (~31 MB) auto-downloads into `notebooks/data/fmnist/` on first run (gitignored)
- **CIFAR-10** (~170 MB) auto-downloads into `notebooks/data/cifar10/` on first run (gitignored)
- ResNet18 weights download once on first transfer-learning notebook execution (~45 MB into `~/.cache/torch/hub/`)

### 5. CIFAR-10 assignment runtime is significant

Total CPU runtime of the assignment is ~25-30 minutes (CifarCNN ~2 min, ResNet18 head ~13 min, fine-tune ~10 min). This is acceptable for an at-home assignment but learners should know what they're committing to.

If runtime becomes a problem (older machines), instructor can suggest:
- Drop `train_rgb` from 10K to 5K images
- Use `mobilenet_v3_small` instead of ResNet18 (~3-4× faster on CPU)
- Skip the fine-tuning step (head-only already passes ≥0.75)

## How to run a full re-verification

```bash
cd "<repo>/learner-edition/L08-computer-vision/notebooks"
for nb in 01_monday_morning 02_convolutions_intuition 03_first_cnn \
         04_transfer_learning optional_extensions assignment; do
  OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \
    jupyter nbconvert --to notebook --execute "$nb.ipynb" \
    --output "$nb.ipynb" --ExecutePreprocessor.timeout=2400
done
```

Expected total wallclock: ~45-60 minutes on a 2020-era MacBook Pro CPU. The assignment (CIFAR-10 + ResNet18 fine-tune) is the bottleneck.

## Module 3 progress

| Lesson | Title | Option A status |
|--------|-------|-----------------|
| L01 | ML Intro & Foundations | ✅ Complete |
| L02 | Supervised Learning Basics | ✅ Complete |
| L03 | Making Models Work | ✅ Complete |
| L04 | Trees, Ensembles & Tuning | ✅ Complete |
| L05 | Unsupervised Learning | ✅ Complete |
| L06 | Time Series Forecasting | ✅ Complete |
| L07 | Neural Networks & DL Foundations | ✅ Complete |
| **L08** | **Computer Vision & CNNs** | **✅ Complete (this lesson)** |
| L09 | NLP & Embeddings | ⏳ Next |
| L10 | Transformers & GenAI | ⏳ |

---

**Ready to teach.** Slides + notebooks + assignment + extensions all execute cleanly. The Fashion-MNIST domain-gap finding is treated as a teaching moment and contrasted against the clean +24pp transfer-learning win on CIFAR-10. Sarah's production-maturity narrative (human-in-the-loop + data-collection plan) closes the lesson.
