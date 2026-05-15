# Lesson 7 — Neural Networks & Deep Learning
*Concept reference. Open whenever you want to look up a definition or check the mechanics used in the notebooks.*

> **Where Sarah is.** Marcus's brief from end-of-L06: *"Predict checkout completion from sequential customer behaviour."* Sarah opens a new dataset — 8,000 customer sessions, 9 behavioural features, target = did the customer complete checkout. The task is tabular binary classification (same family as L03/L04), but this week she does it with a **neural network** to build intuition for L08–L10.

> **The goal of L07 is INTUITION, not production engineering.** You learn what a perceptron is, what multi-layer networks compute, how gradient descent + backprop drive learning, and how to write a PyTorch training loop. That's the foundation for everything in L08 (vision), L09 (NLP), L10 (transformers).

---

## The Big Picture — what neural networks actually are

Strip away the mystique:

> **A neural network is a function — a flexible, non-linear function — whose parameters are learned by gradient descent on a loss function.**

That's it. Everything else is engineering.

Three things to understand:

1. **The function** — how the network maps input to output (forward pass)
2. **The loss** — how we measure "wrong" so we know which direction to adjust
3. **The learning** — gradient descent + backprop, how the parameters get updated

---

## Part 1 — Perceptron → Multi-Layer Perceptron

### The perceptron

The simplest neural unit. It takes inputs, multiplies each by a weight, sums them, adds a bias, and passes the result through an activation function.

```
output = activation( w₁·x₁ + w₂·x₂ + … + wₙ·xₙ + b )
```

With a **sigmoid** activation, that's logistic regression. With a **threshold** activation, that's the classical 1958 perceptron. The point: a single perceptron can only learn LINEARLY SEPARABLE patterns.

### Why one layer isn't enough — the XOR problem

```
Input  Output
(0, 0)   0
(0, 1)   1
(1, 0)   1
(1, 1)   0
```

There's no straight line that separates the 1s from the 0s. A single perceptron CANNOT learn XOR. This was the famous Minsky & Papert critique that froze neural-network research for a decade.

### The fix — multiple layers + non-linear activations

Stack perceptrons in layers. The first layer ("hidden layer") learns intermediate representations; the second layer combines them. With a **non-linear activation** between layers, the network can learn any function (the *universal approximation theorem*).

```
Input → Hidden Layer (e.g., 32 neurons, ReLU activation) → Output Layer (1 neuron, sigmoid for binary classification)
```

The *Multi-Layer Perceptron* (MLP) — also called a *fully-connected* or *dense* network — is just stacked layers of perceptrons with activations in between.

### Activation functions

| Activation | Formula | When |
|---|---|---|
| **ReLU** | `max(0, x)` | Default for hidden layers. Fast, simple, works. |
| **Sigmoid** | `1 / (1 + e⁻ˣ)` | Output layer for binary classification (gives probability) |
| **Softmax** | `eˣⁱ / Σ eˣⱼ` | Output layer for multi-class classification |
| **Tanh** | `(eˣ − e⁻ˣ) / (eˣ + e⁻ˣ)` | Sometimes used in hidden layers (older networks) |

Without a non-linear activation, stacking layers is pointless — multiple linear functions composed are still linear.

### Sklearn's MLPClassifier — the quickest way to train an MLP

```python
from sklearn.neural_network import MLPClassifier

mlp = MLPClassifier(
    hidden_layer_sizes=(32, 16),   # two hidden layers: 32 then 16 neurons
    activation="relu",
    max_iter=500,
    random_state=42,
)
mlp.fit(X_train, y_train)
preds = mlp.predict(X_test)
```

Useful for a quick baseline. For real work — and for L08–L10 — you use PyTorch.

---

## Part 2 — Gradient Descent + Backpropagation (intuition)

### The mountain analogy

Imagine you're blindfolded on a mountain and want to find the lowest point. You can't see, but you can FEEL the slope under your feet.

**Strategy:** at each step, take a small step in the steepest downhill direction. Repeat. Eventually you reach a valley.

That's **gradient descent**:
- The mountain = the loss function
- Your position = the model's parameters
- The slope = the gradient of the loss with respect to parameters
- The step size = the *learning rate*

```
weights_new = weights_old - learning_rate × gradient_of_loss
```

A high learning rate = big steps. Risk: overshoot the valley. A low learning rate = small steps. Risk: takes forever or gets stuck.

### The loss function

For binary classification, the standard loss is **binary cross-entropy** (BCE):

```
BCE = - [ y · log(p) + (1 - y) · log(1 - p) ]
```

Where `y` is the true label (0 or 1) and `p` is the model's predicted probability. BCE is minimised when the predicted probability matches the true label.

In PyTorch: `nn.BCELoss()` or `nn.BCEWithLogitsLoss()` (the latter is numerically more stable — combines sigmoid + BCE in one step).

### What backpropagation does

Gradient descent needs the gradient of the loss with respect to every parameter. For a multi-layer network, there are thousands of parameters. **Backpropagation** is the algorithm that computes all these gradients efficiently using the chain rule.

You don't need to derive backprop by hand. PyTorch (and every other deep learning framework) handles it automatically via **autograd**: track the computation graph during the forward pass, then walk backwards through it to compute gradients.

```python
loss = criterion(predictions, targets)
loss.backward()    # ← this is backprop. One line. PyTorch does the math.
```

### The training loop

The skeleton of every deep learning project:

```
for each epoch:
    for each batch:
        1. predictions = model(inputs)           # forward pass
        2. loss = loss_function(predictions, targets)
        3. optimizer.zero_grad()                 # clear old gradients
        4. loss.backward()                       # backprop
        5. optimizer.step()                      # update weights
```

That's it. The same six lines drive training for an MLP, a CNN, a transformer. The differences live in the model architecture and the loss function.

---

## Part 3 — PyTorch Mechanics

### The four PyTorch objects

| Object | What it does |
|---|---|
| **`Tensor`** | An array with autograd. The atomic data type. Replaces NumPy arrays during model training. |
| **`nn.Module`** | The base class for any model. You subclass it and define `__init__` (layers) + `forward` (compute output from input). |
| **`Dataset` + `DataLoader`** | Wrap your data for efficient batched iteration. |
| **Optimizer** | `torch.optim.Adam`, `SGD`, etc. Knows how to update parameters given gradients. |

### A minimal MLP in PyTorch

```python
import torch
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, n_features, hidden=32):
        super().__init__()
        self.layers = nn.Sequential(
            nn.Linear(n_features, hidden),
            nn.ReLU(),
            nn.Linear(hidden, hidden),
            nn.ReLU(),
            nn.Linear(hidden, 1),     # 1 output neuron for binary classification
        )

    def forward(self, x):
        return self.layers(x)         # returns raw logits

model = MLP(n_features=9, hidden=32)
print(model)
print(f"Total parameters: {sum(p.numel() for p in model.parameters()):,}")
```

### A minimal training loop

```python
from torch.utils.data import DataLoader, TensorDataset

# 1. Wrap data
train_loader = DataLoader(
    TensorDataset(X_train_tensor, y_train_tensor),
    batch_size=64, shuffle=True,
)

# 2. Loss + optimiser
criterion = nn.BCEWithLogitsLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

# 3. Training loop
for epoch in range(20):
    model.train()
    for X_batch, y_batch in train_loader:
        optimizer.zero_grad()
        logits = model(X_batch).squeeze(-1)
        loss = criterion(logits, y_batch.float())
        loss.backward()
        optimizer.step()

    # Evaluate at end of epoch
    model.eval()
    with torch.no_grad():
        val_logits = model(X_val_tensor).squeeze(-1)
        val_loss = criterion(val_logits, y_val_tensor.float()).item()
        val_acc = ((torch.sigmoid(val_logits) > 0.5) == y_val_tensor).float().mean().item()
    print(f"Epoch {epoch+1}: train_loss={loss.item():.3f}, val_loss={val_loss:.3f}, val_acc={val_acc:.3f}")
```

### Common pitfalls

- **Forgetting `optimizer.zero_grad()`** — gradients accumulate by default. Without zeroing, your "gradient" is the sum of all previous batches' gradients.
- **Forgetting `model.eval()` / `model.train()`** — affects dropout and batch norm. Wrong mode = wrong predictions.
- **Using `BCELoss` without manually applying sigmoid** — `BCELoss` expects probabilities (post-sigmoid), `BCEWithLogitsLoss` expects raw logits. Mixing them up silently breaks training.
- **Not setting `random_state`** — neural networks are sensitive to initialisation. Set both `torch.manual_seed(42)` AND any data-shuffling seed.

### Adam vs SGD

| Optimiser | When |
|---|---|
| **SGD** | Simple, classical. Often needs careful learning-rate tuning and a schedule. |
| **Adam** | Adaptive per-parameter learning rate. Default choice for most tabular and NLP problems. |
| **AdamW** | Adam with proper weight decay. Standard for transformers. |

For this lesson, use **Adam with `lr=1e-3`** — almost always reasonable.

---

## Part 4 — When to use neural networks (vs L04 gradient boosting)

| Property | MLP shines | Gradient Boosting shines |
|---|---|---|
| **Tabular data, moderate size (10k–1M rows)** | Sometimes wins | Usually wins |
| **Tabular data, big (>10M rows)** | Often wins | Slower, may saturate |
| **Image / text / audio** | Wins decisively | Doesn't really apply |
| **Sparse high-dim features** | Wins | Can struggle |
| **Need interpretable feature importance** | Hard | Built-in |
| **Need fast training + few hyperparameters** | Worse | Better |
| **Need state-of-the-art on benchmarks** | Sometimes | Sometimes |

**Rule of thumb for L07's use case (tabular session data):** start with gradient boosting (L04). Only move to MLPs if you have a specific reason (much larger data, need to ensemble with other neural nets, etc.) or if you're benchmarking.

L07's REAL value is the foundation for L08–L10, where neural networks are the only practical choice.

---

## Friday — what Sarah presents

Sarah's session-completion model is now a comparison table:

| Model | Test AUC | When to use |
|---|---|---|
| Logistic Regression (L03 baseline) | ~0.761 | Most interpretable |
| Gradient Boosting (L04 toolkit) | ~0.747 | Usually wins on tabular — but didn't here |
| sklearn MLP | ~0.761 | Tied with LR |
| PyTorch MLP | ~0.756 | Foundation for deep learning |

**Honest read:** on Sarah's session-completion task all four models land within 0.015 AUC of each other. Logistic regression and the sklearn MLP are statistically indistinguishable; gradient boosting is the *lowest* of the four. This is what tabular ML actually looks like — the algorithm rarely matters as much as the features.

The right deployment choice for THIS task is probably logistic regression (cheapest, most interpretable, top AUC). The MLP work pays off in L08-L10 where we leave tabular data behind.

Marcus listens, nods, then asks:

> *"OK. Now — we're starting an Instagram campaign. Can you automatically tag product PHOTOS so they auto-categorise as 'dress', 'jeans', 'jacket', etc.?"*

That question — **prediction from images** — is the engine of **L08 (Computer Vision).**

---

## Glossary

See [`reference.md`](./reference.md) for a 20-term glossary covering perceptrons, activations, gradient descent, PyTorch, and deep-learning vocabulary.
