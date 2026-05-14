# L08 — Lesson · Computer Vision & Convolutional Neural Networks

> *Sarah's NN model from L07 shipped. The team is unblocked on checkout-completion prediction. Then Marcus walks over with the next ask: auto-tag product photos. Same network playbook, very different input.*

## Part 1 — Why images break MLPs

### 1.1 An image is a tensor

A grayscale 28×28 image is a tensor of shape `(28, 28)`. A colour 224×224 photograph is `(3, 224, 224)` — three channels (red, green, blue), each with 224×224 pixel values. PyTorch's convention is `(batch, channels, height, width)`.

A neural network only sees numbers, so an image is just **a particular shape of array** filled with pixel intensities between 0 and 255 (or 0.0 and 1.0 after normalisation).

### 1.2 The flat-MLP problem

The simplest thing to try is what we did in L07: flatten the image into a vector, pass it through an MLP.

For 28×28 Fashion-MNIST:
- Input dim: 784
- Hidden layer 1: 256 neurons → **200,960 parameters** in that one layer
- Hidden layer 2: 128 → 32,896 more
- Output: 10 classes → 1,290 more
- **Total: ~235K parameters** for tiny 28×28 images

Bump the image to a realistic 224×224 colour photo (3×224×224 = 150,528 input features) and the first hidden layer alone needs ~38 million parameters. That's untenable.

### 1.3 The spatial-locality problem

There's a worse problem than parameter count: an MLP treats every input pixel as **independent of every other pixel**. The model has no built-in notion that pixel `(10, 10)` is right next to pixel `(10, 11)`. It has to *learn* that nearby pixels are related, from training data. That's expensive and fragile.

Real images have two properties an MLP can't exploit:
1. **Locality** — nearby pixels are related (edges, corners, textures live in small patches).
2. **Translation invariance** — a sleeve is a sleeve whether it's in the top-left or bottom-right of the image.

We need a model that bakes both of these in. That model is the **convolutional neural network**.

## Part 2 — Convolutions

### 2.1 The kernel intuition

A convolution slides a small window — a **kernel** (also called a **filter**) — across the image, computing a weighted sum at each location.

Take this 3×3 edge-detection kernel:

```
-1  -1  -1
-1   8  -1
-1  -1  -1
```

When you slide it over a flat region of an image (all pixels similar), the centre pixel cancels the eight neighbours → output ≈ 0. When you slide it over an edge (centre pixel different from neighbours), output is large. So this single 3×3 kernel — just **9 numbers** — produces a feature map that highlights edges across the entire image.

### 2.2 The parameter saving

Here's the magic: that 9-parameter kernel slides everywhere. To find a sleeve-edge feature in a 224×224 image, an MLP needs separate weights for "edge at top-left" and "edge at bottom-right". A convolution learns the edge detector **once** and reuses it everywhere.

A single `Conv2d(in_channels=3, out_channels=32, kernel_size=3)` layer has:

- 32 kernels × (3 channels × 3 × 3) = **864 weights** + 32 biases = **896 parameters**

For comparison: an MLP first layer on the same input with 32 outputs needs `3·224·224·32 ≈ 4.8 million` parameters. The convolution is **~5,000× smaller**.

### 2.3 Stride, padding, pooling

- **Stride** — how many pixels the kernel jumps between applications. Stride 2 halves the output size.
- **Padding** — adding a border of zeros around the input so the kernel can reach the edges and the output keeps the same size.
- **Max pooling** — after a conv layer, take the max over each 2×2 block. Shrinks the feature map, throws away precise location, keeps the strongest activation. Classic CNN pattern: `Conv → ReLU → MaxPool`.

### 2.4 Stacking conv layers

One conv layer learns simple features (edges, blobs). Stack a second on top of it and you learn combinations of those features (corners, sleeve curves). Stack a third and you learn whole-object patterns (a shirt collar, a sneaker laces). This compositionality is why deep CNNs work.

## Part 3 — Building a CNN in PyTorch

### 3.1 The minimum architecture

A working CNN for Fashion-MNIST needs just five blocks:

```python
class TinyCNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(1, 16, kernel_size=3, padding=1),   # 1×28×28 → 16×28×28
            nn.ReLU(),
            nn.MaxPool2d(2),                              # 16×28×28 → 16×14×14
            nn.Conv2d(16, 32, kernel_size=3, padding=1),  # 16×14×14 → 32×14×14
            nn.ReLU(),
            nn.MaxPool2d(2),                              # 32×14×14 → 32×7×7
        )
        self.classifier = nn.Sequential(
            nn.Flatten(),
            nn.Linear(32 * 7 * 7, 64),
            nn.ReLU(),
            nn.Linear(64, 10),
        )
    def forward(self, x):
        return self.classifier(self.features(x))
```

That's ~125K parameters, half the MLP's count, and we'll see it pulls ~91% test accuracy — meaningfully better than the MLP's ~85%.

### 3.2 The training loop is identical to L07

The PyTorch training-loop pattern from L07 (`zero_grad → forward → loss → backward → step`) works unchanged. CNNs aren't a different framework, they're a different *architecture* dropped into the same loop.

## Part 4 — Transfer learning

### 4.1 The problem with small data

Sarah's actual photo collection is 500 images per category, not 60,000. Train a CNN from scratch on 500 images and you'll over-fit aggressively. So what do you do when you have a tiny dataset?

**Don't train from scratch. Start from a model someone else already trained.**

### 4.2 What pretrained means

Researchers have trained big CNNs (ResNet, EfficientNet, ViT) on ImageNet — 1.2 million labelled photos across 1,000 categories. Those models have learned a hierarchy of visual features: edges, textures, object parts, whole-object detectors.

Most of that hierarchy is **task-independent**. Sleeve detectors and texture filters trained on ImageNet work just fine on Fashion-MNIST.

### 4.3 The two transfer-learning patterns

**Feature extraction** — freeze the pretrained backbone, replace only the final classification head with a new one for your classes, train only that head. Works great when your data is small and similar to the pretraining domain.

**Fine-tuning** — same as above, but after a few epochs of head-only training, *unfreeze* the last few backbone layers and continue training with a tiny learning rate. Squeezes out the last few accuracy points when you have somewhat more data.

### 4.4 The honest expectation

Transfer learning isn't free. It wins when the **pretrained domain is close to your domain**. ImageNet → product catalogue photos (colour, natural) is a great match. ImageNet → Fashion-MNIST (grayscale silhouettes) is a poor match — head-only transfer can actually underperform a from-scratch CNN there.

When the domain gap is wide, **don't just train the head — fine-tune**. Unfreeze the last conv block (`layer4` in ResNet), use a tiny learning rate (1e-4), and let the deepest features adapt to your data. That's where transfer learning recovers its reputation. NB 04 walks through both outcomes back to back so you can see the difference.

## Part 5 — When to use what

| Scenario | Recommendation |
|----------|----------------|
| Tabular features (rows × columns) | Stick with gradient boosting or logistic regression (see L04, L02) |
| 10K+ small images, 10 classes | Train a small CNN from scratch |
| < 1K images per class | Transfer learning with a pretrained backbone |
| 100+ classes, lots of data | Fine-tune a larger pretrained model |
| Production scale, need fast inference | Compile to ONNX or use a smaller backbone (MobileNet) |
| Need it Tuesday | Call a hosted vision API and revisit later |

## Part 6 — Sarah's recommendation to Marcus

Sarah's verdict for the catalogue auto-tagger:

> *"Start with transfer learning on a pretrained ResNet18 and **fine-tune the last conv block**, not just the head. Our product photos are colour and natural-photo-like, so the ImageNet pretraining transfers well — but only after we let the deepest layer adapt to clothing. From-scratch CNN would need 10× more labelled data and still might not catch up on shirt/T-shirt/coat ambiguity."*

She also flags the production reality:

> *"For the launch, we should auto-tag with the model AND surface the top-3 predictions to merchandisers for confirmation. Treat the model as a first-pass labeller, not a final source of truth. The merchandisers stay in the loop until we trust the model's confidence calibration."*

This is the right pattern. Models in production should know when to defer.

## Bridge to L09

Marcus's next question after the photo tagger ships: *"Can we also do natural-language search? A customer types 'blue summer dress' and we surface the right products?"*

Same neural-network family, completely different input modality. Pixels → words. We need to learn how to turn text into vectors, and how to compare those vectors. That's **embeddings** and the beginning of L09.

---

## Recap — three things to remember

1. **Convolutions exploit locality and translation invariance.** Same kernel slides everywhere → fewer parameters, better inductive bias for images.
2. **The PyTorch training loop doesn't change.** Conv2d/MaxPool2d are drop-in replacements for Linear. The training pattern from L07 still works.
3. **Don't train CNNs from scratch on small data.** Use a pretrained backbone. Transfer learning is the default for any new image task.
