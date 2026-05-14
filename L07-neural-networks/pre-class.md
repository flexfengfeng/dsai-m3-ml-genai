# Pre-class — L07 Neural Networks

**Time:** about 75 minutes. Do this before the live session.

**Goal:** Build intuition for what a neural network IS before we start training one in class.

---

## What you're walking into

End of L06 Marcus said:

> *"If we collected DETAILED CUSTOMER BEHAVIOUR DATA (every click, every page view, every cart event), could you predict whether they'd complete checkout WHILE THEY'RE SHOPPING?"*

This week Sarah has `northstar_sessions.csv` — 8,000 customer sessions with 9 behavioural features and a binary target (`completed`: did they finish checkout?). 

She'll train **three models** and compare:
1. Logistic regression (L03 baseline)
2. Gradient Boosting (L04 toolkit)
3. **Multi-Layer Perceptron in PyTorch** (this week's new tool)

The lesson goal is **understanding what neural networks do** — not winning the leaderboard. Real-world Sarah might ship gradient boosting on a problem like this. The MLP is the foundation for L08 (images), L09 (text), and L10 (transformers).

---

## Task 1 (~20 min) — Open `01_monday_morning.ipynb`

This notebook loads the session data and runs a quick logistic-regression baseline. You see what tabular classification looks like before we add neural networks.

**Open** `notebooks/01_monday_morning.ipynb` and **run every cell**.

As you go, jot down:
- The completion rate (% of sessions that complete checkout)
- Which two features look most correlated with completion
- The baseline accuracy + AUC

---

## Task 2 (~30 min) — Watch two videos

### Video 1 — But what is a Neural Network? (3Blue1Brown, 19 min)

[https://www.youtube.com/watch?v=aircAruvnKk](https://www.youtube.com/watch?v=aircAruvnKk)

Why it matters: the single best visual explanation of what a neural network actually IS. By the end of 19 minutes you'll understand the function/parameters/output relationship that the next 4 hours of code-along is about.

**Mini-exercise:** in your own words, what does a single neuron compute?

> **Sample answer:** A neuron takes its inputs (features or previous-layer activations), multiplies each by a learned weight, sums them with a bias, then passes the result through a non-linear activation function (e.g., ReLU). The output is one number that becomes input to the next layer.

### Video 2 — Gradient descent (3Blue1Brown, 21 min)

[https://www.youtube.com/watch?v=IHZwWFHWa-w](https://www.youtube.com/watch?v=IHZwWFHWa-w)

Why it matters: gradient descent + backprop is HOW networks learn. Without this mental model, the PyTorch training loop is magic.

**Mini-exercise:** what does "the loss is a function of the parameters" mean?

> **Sample answer:** The loss tells us how wrong the model is. The model's wrongness depends on its weights. So for ANY set of weights you could imagine, there's a corresponding loss. The 'landscape' of loss as a function of all the weights is what gradient descent walks down. The lowest point in that landscape is the trained model.

---

## Task 3 (~25 min) — Quick exercises

### Exercise 1 — XOR by hand

Why does this dataset need a multi-layer network?

| x₁ | x₂ | y |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Try to draw a single straight line on the (x₁, x₂) plane that separates the y=0 points from the y=1 points. Can you?

> **Sample answer:** No straight line works. y=1 points are in opposite corners; y=0 points are in the other two corners. This is the famous XOR problem. A single perceptron (which can only learn linear boundaries) cannot solve XOR. A multi-layer network with at least one hidden layer + a non-linear activation CAN.

### Exercise 2 — Where do parameters live?

A network has:
- Input: 9 features (Sarah's session data)
- Hidden layer 1: 32 neurons
- Hidden layer 2: 16 neurons
- Output: 1 neuron (probability of completion)

How many WEIGHT parameters does this network have? (Ignore biases for simplicity.)

> **Sample answer:**
> - Input to hidden 1: 9 × 32 = 288 weights
> - Hidden 1 to hidden 2: 32 × 16 = 512 weights
> - Hidden 2 to output: 16 × 1 = 16 weights
> - **Total weights: 816** (plus biases: 32 + 16 + 1 = 49 biases → grand total 865 parameters)
>
> With ~8,000 training examples, this is a reasonable model size — neither too small nor too large.

### Exercise 3 — Why we need `optimizer.zero_grad()`

In PyTorch, gradients ACCUMULATE by default — each `.backward()` call ADDS to the existing gradients. If you forget `optimizer.zero_grad()` before each batch, what would happen?

> **Sample answer:** Each batch's gradient would include the sum of all previous batches' gradients in the epoch. The "gradient step" would become huge and wildly off-direction. Training would diverge — loss would explode or oscillate, instead of decreasing. `optimizer.zero_grad()` resets gradients to zero before each batch so each backward pass produces a CLEAN gradient for just that batch.

---

## Active-engagement tips

- **You won't derive backprop in class.** The mental model is what matters. PyTorch does the math.
- **Start small with neural networks.** L07's MLP has hundreds of parameters, not millions. Build intuition before scaling up in L08.

---

## Bring to the session

1. ✅ Run `01_monday_morning.ipynb` end-to-end
2. ✅ Watched both 3Blue1Brown videos
3. ✅ Three exercise answers ready
4. ✅ One question that didn't click

See you Tuesday.
