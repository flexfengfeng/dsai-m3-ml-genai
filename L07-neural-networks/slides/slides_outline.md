# L07 — Neural Networks & Deep Learning Foundations · Slide Outline

**Deck:** `L07_slides.pptx` · 28 slides · Ocean Gradient palette
**Total runtime:** ~90 min of slide-driven lecture, interleaved with three code-along checkpoints in the notebooks.

---

## Section 1 — Opening & Recap (slides 1–3)

| # | Slide | Purpose |
|---|-------|---------|
| 1 | **Title** — "Neural Networks & Deep Learning Foundations" | Set the room. Sarah is now four lessons into her ML journey. |
| 2 | **Where we are** | Map showing L01→L07 path. L06 closed with Marcus asking: *"Can you predict checkout completion from a customer's session behaviour?"* |
| 3 | **Today's three questions** | (1) What IS a neural network? (2) How does it learn? (3) When should we reach for it vs gradient boosting? |

## Section 2 — What Is a Neural Network? (slides 4–8)

| # | Slide | Purpose |
|---|-------|---------|
| 4 | **The perceptron** | One neuron: weighted sum → activation → output. The 1958 ancestor of every modern net. |
| 5 | **The MLP** | Stack neurons into layers. Input → hidden(s) → output. "Multi-Layer Perceptron." |
| 6 | **The XOR problem** | Why linear models fail on `XOR` and why ONE hidden layer fixes it. The historical moment that almost killed NN research. |
| 7 | **Activation functions** | ReLU, sigmoid, tanh. Why nonlinearity matters. Why ReLU won. |
| 8 | **Code-along checkpoint: Notebook 02** | "Open `02_perceptron_to_mlp.ipynb` — let's watch a perceptron fail at XOR, then watch an MLP solve it." |

## Section 3 — How Does It Learn? (slides 9–13)

| # | Slide | Purpose |
|---|-------|---------|
| 9 | **Gradient descent — the mountain analogy** | You're on a foggy mountain. You can only feel the slope under your feet. How do you get to the valley? |
| 10 | **The update rule** | `w ← w − η · ∂L/∂w`. One equation that rules them all. Why η (learning rate) is the dial that breaks everything. |
| 11 | **Backpropagation** | Chain rule, layer by layer, output → input. Why this is the algorithm. |
| 12 | **Loss landscape intuition** | Convex vs non-convex. Why NN training is not guaranteed to find the global minimum — and why we don't care in practice. |
| 13 | **Code-along checkpoint: Notebook 03** | "Open `03_gradient_descent.ipynb` — manual gradient descent on `(w−3)²`, then PyTorch autograd, then a tiny MLP trained from scratch." |

## Section 4 — PyTorch in 4 Objects (slides 14–18)

| # | Slide | Purpose |
|---|-------|---------|
| 14 | **The four objects you need to know** | Tensor · `nn.Module` · `DataLoader` · `Optimizer`. That's it. |
| 15 | **Tensor** | Like a NumPy array, but tracks gradients. `requires_grad=True`. |
| 16 | **`nn.Module` & `Optimizer`** | Define your model. Pick your optimizer (Adam by default). |
| 17 | **The PyTorch training loop** | The five lines that show up in every PyTorch project: zero_grad → forward → loss → backward → step. |
| 18 | **Code-along checkpoint: Notebook 04** | "Open `04_pytorch_training_loop.ipynb` — production training loop with DataLoader, Adam, and early stopping." |

## Section 5 — MLP vs Gradient Boosting (slides 19–22)

| # | Slide | Purpose |
|---|-------|---------|
| 19 | **The honest comparison** | On Sarah's session data: LR 0.761, GB 0.747, sklearn MLP 0.761, PyTorch MLP 0.756. *They are TIED.* |
| 20 | **The "neural nets are always best" myth** | On tabular data, gradient boosting often wins or ties. Kaggle leaderboards confirm. |
| 21 | **So why learn NNs?** | Because in L08 we'll see images, in L09 text. THIS is the foundation for everything that comes next. |
| 22 | **Sarah's Friday recommendation** | Ship gradient boosting for the website (mature, fast, interpretable). Use NN insight to inform feature engineering. |

## Section 6 — Recap & Bridge (slides 23–28)

| # | Slide | Purpose |
|---|-------|---------|
| 23 | **Three things to remember** | (1) NN = stacked nonlinear functions. (2) Gradient descent = the universal training recipe. (3) PyTorch = 4 objects, 5 lines. |
| 24 | **Sarah's journey so far** | L01→L07. Visual showing tools mastered: LR · trees · clustering · time series · NN. |
| 25 | **Method comparison cheat sheet** | When to use what (tabular vs sequence vs image vs text). |
| 26 | **Common pitfalls** | Learning rate too high, no normalisation, training too long, ReLU dying, batch size too small. |
| 27 | **Bridge to L08** | Marcus's next question: *"Can you automatically tag product photos as dress, jeans, jacket?"* → Computer Vision & CNNs. |
| 28 | **Q&A · Assignment preview** | Loan default classifier + housing price regressor (both in PyTorch). |

---

## Embedded code-along checkpoints

Slides 8, 13, and 18 are explicit handoffs to the notebooks. Each tells learners which notebook to open and what to expect. Plan ~15-20 minutes per checkpoint.

## Design notes

- **Palette:** Ocean Gradient (NAVY `#065A82`, TEAL `#1C7293`, MIDNIGHT `#21295C`, with CREAM `#F5F5F5` for body)
- **Header font:** Georgia (titles 36–40pt). **Body font:** Calibri (16–20pt)
- **Visual motif:** Every concept slide has a sketch panel on the right (neuron diagram, XOR scatter, mountain illustration, training loop arrows, etc.)
- **No AI-tell accent lines** under titles — whitespace and background colour blocks instead
