# L07 — Neural Networks & Deep Learning

> *Sarah Chen's seventh week at NorthStar Retail. Marcus's question from L06: "Can you predict checkout completion FROM SEQUENTIAL CUSTOMER BEHAVIOUR while they're shopping?" This week she trains her first neural network.*
> By the end of this lesson you will know what a perceptron is, what a multi-layer network does, how gradient descent + backprop make learning work, and how to write a PyTorch training loop end-to-end.

---

## Before you start — environment setup

> **If this is your first time, follow the [Setup Guide →](./setup.md)** (installs PyTorch).
>
> **Already set up from L01–L06?** Run `pip install torch` once — L07 introduces PyTorch as a new dependency.

---

## Where L07 fits

| Lesson | What you build |
|---|---|
| **L01–L05** | Classical ML — supervised + unsupervised on tabular data |
| **L06 — Time Series** | Forecasting on temporal data |
| **L07 — Neural Networks** *(you are here)* | First neural network: MLP for tabular classification + PyTorch training loop |
| **L08 — Computer Vision** | CNN — neural networks for images |
| **L09 — NLP** | Neural networks for text |
| **L10 — Transformers & GenAI** | The architecture behind ChatGPT, BERT, etc. |

**The narrative thread:** L07 is the **bridge** from classical ML to deep learning. The goal is intuition — what a neural network IS, how it LEARNS, and how to train one in PyTorch — not production engineering. The next three lessons specialise this toolkit to images (L08), text (L09), and transformers (L10).

---

## What you will be able to do by the end

- **Describe** what a perceptron is, why adding multiple layers + non-linearity is needed for non-linear problems
- **Explain** gradient descent + backpropagation in plain English (no chain-rule derivation required)
- **Write** a PyTorch training loop end-to-end — `Dataset` → `DataLoader` → model → loss → optimiser → forward → backward → step
- **Train** a simple multi-layer perceptron and reach a non-trivial accuracy on Sarah's session-completion data
- **Compare** the MLP to L03/L04 baselines (logistic regression, gradient boosting) — and have an opinion on when each is appropriate

---

## How class time is structured (~3 hrs)

| Phase | Time | Format |
|---|---|---|
| Slide-driven lecture | ~90 min | Instructor uses [`slides/L07_slides.pptx`](./slides/L07_slides.pptx) (~28 slides) |
| Hands-on code-alongs | ~90 min | Three notebooks (~30 min each) |
| (Self-study after class) | self-paced | Each notebook has 🟡 Extension + the assignment |

---

## Your learning path

### Phase 1 — Before class: self-study (~75 min)

**Start here →** [**pre-class.md**](./pre-class.md)

You'll run `01_monday_morning.ipynb` to see the customer-session data + watch a video on neural networks + try three mini-exercises.

---

### Phase 2 — In class: hands-on (~3 hrs)

**Concept reference →** [**lesson.md**](./lesson.md)

| # | Notebook | Sarah's day | What you explore |
|---|---|---|---|
| 02 | [`02_perceptron_to_mlp.ipynb`](./notebooks/02_perceptron_to_mlp.ipynb) | Tuesday | Perceptron · why non-linearity is needed · sklearn MLPClassifier baseline |
| 03 | [`03_gradient_descent.ipynb`](./notebooks/03_gradient_descent.ipynb) | Wednesday | Gradient descent intuition · backprop mental model · simple PyTorch model |
| 04 | [`04_pytorch_training_loop.ipynb`](./notebooks/04_pytorch_training_loop.ipynb) | Thursday | Full PyTorch training loop · Adam optimiser · compare to L04 baselines |

---

### Phase 3 — After class: assignment (self-paced)

**Assignment →** [`notebooks/assignment.ipynb`](./notebooks/assignment.ipynb)

Apply the MLP toolkit to a new tabular domain (loan-default prediction).

**Further reading →** [**reference.md**](./reference.md)

---

## Core vs Optional — what this lesson teaches

**🟢 Core (taught in class):**
- Perceptron → multi-layer mental model · activations · loss
- Gradient descent + backpropagation intuition
- PyTorch training loop end-to-end

**🟡 Optional (self-study, not assessed):**
- NumPy-from-scratch neural net (manual forward + backward pass)
- Weight initialisation strategies (Xavier, He)
- Adam vs SGD vs RMSprop comparison
- Dropout + batch normalisation

Optional material lives in [`notebooks/optional_extensions.ipynb`](./notebooks/optional_extensions.ipynb).

---

## File map

```
README.md                              ← You are here
setup.md                               ← One-time environment setup
pre-class.md                           ← Phase 1: 75-min self-study
lesson.md                              ← Phase 2: Concept reference
reference.md                           ← Phase 3: Further reading + glossary
environment.yml                        ← Conda environment spec
slides/
  L07_slides.pptx                      ← Instructor slide deck (~28 slides)
  slides_outline.md                    ← Slide-by-slide outline
notebooks/
  data/
    northstar_sessions.csv             ← 8,000 customer sessions
  01_monday_morning.ipynb              ← Pre-class hook
  02_perceptron_to_mlp.ipynb           ← Part 1: Perceptron → MLP
  03_gradient_descent.ipynb            ← Part 2: GD + PyTorch basics
  04_pytorch_training_loop.ipynb       ← Part 3: Full PyTorch loop + comparison
  assignment.ipynb                     ← After class: loan-default prediction
  optional_extensions.ipynb            ← 🟡 NumPy from scratch · weight init · Adam variants · dropout · batch norm
```
