# L07 — Neural Networks & Deep Learning Foundations · Option A Handoff

**Status:** ✅ Complete
**Build date:** 2026-05-14
**Pattern:** Option A — slide-driven lecture (~90 min) + 4 in-class notebooks (~90 min), 🟡 Extension boundary separating Core from self-study.

---

## What's inside

```
L07-neural-networks/
├── README.md                       Sarah's brief + Phase 1/2/3 structure
├── setup.md                        Environment setup (pip install torch)
├── pre-class.md                    75-min self-study (3Blue1Brown + 3 exercises)
├── lesson.md                       Concept reference, full narrative
├── reference.md                    20-term glossary
├── environment.yml                 Conda env spec
├── slides/
│   ├── L07_slides.pptx             28 slides, Ocean Gradient palette
│   └── slides_outline.md           Slide-by-slide breakdown
├── notebooks/
│   ├── 01_monday_morning.ipynb     Pre-class hook — baseline LR (13 cells)
│   ├── 02_perceptron_to_mlp.ipynb  XOR demo + MLPClassifier (21 cells)
│   ├── 03_gradient_descent.ipynb   Manual GD → PyTorch autograd → first MLP (25 cells)
│   ├── 04_pytorch_training_loop.ipynb  DataLoader + Adam + early stopping (25 cells)
│   ├── assignment.ipynb            Loan default + housing regression (22 cells)
│   ├── optional_extensions.ipynb   NumPy MLP from scratch + variants (11 cells)
│   └── data/
│       └── northstar_sessions.csv  8,000 sessions × 11 features
└── HANDOFF_OPTION_A.md             (this file)
```

## Smoke-test results

All 6 notebooks executed end-to-end with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1`:

| Notebook | Cells | Status | Key result |
|----------|------:|--------|-----------|
| 01_monday_morning            | 13 | ✅ | Baseline LR: Acc 0.703, AUC 0.761 |
| 02_perceptron_to_mlp         | 21 | ✅ | MLP AUC 0.757 (tied with LR — see note below) |
| 03_gradient_descent          | 25 | ✅ | SimpleMLP from scratch: AUC 0.742 |
| 04_pytorch_training_loop     | 25 | ✅ | PyTorch MLP: AUC 0.756; four-way comparison table |
| assignment                   | 22 | ✅ | Loan: AUC 0.771; Housing: MAE $31.9k |
| optional_extensions          | 11 | ✅ | NumPy-from-scratch MLP + 3 variants |

## Narrative beats

1. **L06 → L07 bridge:** Marcus's Friday question carries over — "Can you predict checkout completion from sequential session behaviour?"
2. **The XOR moment:** A single perceptron achieves 50% accuracy on XOR. One hidden layer takes it to 100%. The historical pivot that justified everything.
3. **Gradient descent demystified:** Mountain analogy → manual GD on `(w−3)²` → PyTorch autograd → real model.
4. **PyTorch in 4 objects:** Tensor, `nn.Module`, `DataLoader`, `Optimizer`. The 5-line training loop pattern.
5. **The honest comparison:** On Sarah's tabular data, LR / GB / sklearn MLP / PyTorch MLP all land within 0.015 AUC of each other. **No magic happens here.** The point is foundation for L08–L10.
6. **L07 → L08 bridge:** Marcus's next question — "Can you automatically tag product PHOTOS as dress, jeans, jacket?" → CNNs in L08.

## Deviations & notes for the instructor

### 1. PyTorch threading: required for stability

**Symptom:** Default OpenMP threading caused `nbconvert --execute` on NB04 to exit with code 139 (segfault) intermittently.

**Fix applied:** All smoke tests are run with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1`. The notebooks themselves also call `torch.set_num_threads(1)` at the top of each PyTorch cell.

**Instructor action:** If a learner reports an unexplained kernel crash, ask them to run `OMP_NUM_THREADS=1 jupyter notebook` instead of `jupyter notebook` on macOS. (No issue observed on Linux or Windows.)

### 2. MLP ties LR on this dataset — and that's the lesson

**The honest finding:** On `northstar_sessions.csv` (8,000 rows, 9 features, mostly linear signal), the MLP does NOT meaningfully beat logistic regression. AUC numbers are essentially tied.

**Why this is right, not wrong:** This is a faithful representation of what happens with NNs on real tabular data. The L07 narrative explicitly calls this out (see NB02 closing cell, slide 19, lesson.md Part 4). The lesson the learner should take away: *NNs are the foundation for non-tabular modalities (images, text, sequences) you'll meet in L08–L10 — on tabular data, gradient boosting often wins.*

**Don't "fix" this** by amplifying nonlinearity in the data — that would teach a false confidence in NNs.

### 3. NB03 SimpleMLP underperforms

The from-scratch SimpleMLP in NB03 lands at AUC 0.742 — a touch below LR. This is deliberate: full-batch SGD for only 200 epochs without normalisation is meant to feel slightly worse, motivating the NB04 upgrade (DataLoader, Adam, early stopping).

### 4. The data is intentionally "boring" for NNs

`northstar_sessions.csv` was designed with some nonlinear interactions (e.g. `pages_viewed × time_on_site` proxies engagement), but the dominant signal is still linear. This was a deliberate choice to teach the honest comparison rather than to produce a flashy NN-wins result.

## How to run a full re-verification

```bash
cd "<repo>/learner-edition/L07-neural-networks/notebooks"
for nb in 01_monday_morning 02_perceptron_to_mlp 03_gradient_descent \
         04_pytorch_training_loop assignment optional_extensions; do
  OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \
    jupyter nbconvert --to notebook --execute "$nb.ipynb" \
    --output "$nb.ipynb" --ExecutePreprocessor.timeout=300
done
```

Expected: all six complete in ≤ 5 minutes total on a 2020-era MacBook Pro.

## Module 3 progress

| Lesson | Title | Option A status |
|--------|-------|-----------------|
| L01 | ML Intro & Foundations | ✅ Complete |
| L02 | Supervised Learning Basics | ✅ Complete |
| L03 | Making Models Work | ✅ Complete |
| L04 | Trees, Ensembles & Tuning | ✅ Complete |
| L05 | Unsupervised Learning | ✅ Complete |
| L06 | Time Series Forecasting | ✅ Complete |
| **L07** | **Neural Networks & DL Foundations** | **✅ Complete (this lesson)** |
| L08 | Computer Vision & CNNs | ⏳ Next |
| L09 | NLP & Embeddings | ⏳ |
| L10 | Transformers & GenAI | ⏳ |

---

**Ready to teach.** Slides + notebooks + assignment + extensions all execute cleanly. The narrative is honest about NN performance on tabular data and sets up L08 cleanly.
