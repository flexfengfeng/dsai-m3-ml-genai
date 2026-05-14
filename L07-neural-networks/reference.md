# L07 — Further Reading & Glossary

## Further reading

### Videos (free)

- **3Blue1Brown — Neural Networks series (4 videos, ~75 min total)**
  [https://www.3blue1brown.com/topics/neural-networks](https://www.3blue1brown.com/topics/neural-networks)
  The best visual intro to neural networks anywhere.

- **PyTorch official tutorials**
  [https://pytorch.org/tutorials/beginner/basics/intro.html](https://pytorch.org/tutorials/beginner/basics/intro.html)
  Tensors, autograd, building neural networks, training loops.

### Books / online

- **Deep Learning with PyTorch** by Eli Stevens et al. — free first chapter; excellent reference.
- **Neural Networks and Deep Learning** by Michael Nielsen — free online book, beautifully explained.
- **Dive into Deep Learning** (d2l.ai) — free interactive textbook covering everything from MLPs to transformers.

### Interactive / official docs

- **PyTorch docs** — [https://pytorch.org/docs/](https://pytorch.org/docs/)
- **Playground** — [https://playground.tensorflow.org/](https://playground.tensorflow.org/) — interactive visualisation of MLPs

---

## Glossary (20 key terms)

- **Activation function** — A non-linear function applied to a neuron's weighted sum (e.g., ReLU, sigmoid, tanh). Without non-linear activations, stacking layers is mathematically equivalent to a single layer.

- **Adam** — An adaptive learning-rate optimiser. Default choice for most tabular and NLP problems. Combines momentum + per-parameter adaptive rates.

- **Autograd** — PyTorch's automatic differentiation engine. Tracks operations during the forward pass; computes gradients on `.backward()` using the chain rule.

- **Backpropagation** — The algorithm that computes gradients of the loss with respect to every parameter, by applying the chain rule backward through the network. PyTorch does this automatically.

- **Batch** — A subset of training examples processed together in one forward + backward pass. Typical sizes: 32, 64, 128.

- **BCEWithLogitsLoss** — Binary cross-entropy loss, computed numerically stably from raw logits (no need to apply sigmoid manually). Default for binary classification in PyTorch.

- **DataLoader** — PyTorch utility that wraps a `Dataset` and provides batched, shuffled iteration.

- **Dropout** — A regularisation technique that randomly zeros some neuron outputs during training. Helps prevent overfitting.

- **Epoch** — One full pass through the entire training set. Usually train for 5-50 epochs depending on data size.

- **Forward pass** — Compute predictions from inputs by feeding them through the network layer by layer.

- **Gradient descent** — The optimisation algorithm. Move parameters in the direction of the NEGATIVE gradient of the loss. The fundamental learning algorithm.

- **Hidden layer** — Any layer between input and output. "Deep" learning means many hidden layers.

- **Learning rate** — How big a step gradient descent takes. Too high → unstable; too low → slow. Typical range: 1e-5 to 1e-2.

- **Logit** — A raw, unbounded score before applying sigmoid or softmax. PyTorch's `BCEWithLogitsLoss` expects logits (not probabilities).

- **Loss function** — A function that measures how wrong the model's predictions are. Binary cross-entropy for binary classification; MSE for regression.

- **MLP (Multi-Layer Perceptron)** — A neural network of stacked fully-connected (dense) layers with non-linear activations. The simplest neural-network architecture.

- **Optimizer** — The PyTorch object (e.g., `torch.optim.Adam`) that updates parameters given gradients. Lives separately from the model.

- **Perceptron** — A single neuron — weighted sum + activation. The 1958 building block from which neural networks were named.

- **ReLU** — Rectified Linear Unit. `f(x) = max(0, x)`. The default activation in modern hidden layers; fast and works well.

- **`zero_grad()`** — Reset accumulated gradients to zero. MUST be called before each batch's `backward()` or gradients accumulate across batches.
