# L08 — Computer Vision & CNNs · Slide Outline

**Deck:** `L08_slides.pptx` · 28 slides · Ocean Gradient palette
**Total runtime:** ~90 min of slide-driven lecture, interleaved with three code-along checkpoints in the notebooks.

---

## Section 1 — Opening & Recap (slides 1–3)

| # | Slide | Purpose |
|---|-------|---------|
| 1 | **Title** — "Computer Vision · CNNs & Transfer Learning" | Set the room. Sarah is now five lessons into her ML journey. |
| 2 | **Where we are** | L07 just shipped a NN. Marcus's L08 ask: "Can we auto-tag the product photos?" |
| 3 | **Today's three questions** | (1) What does an image LOOK LIKE to a NN? (2) What IS a CNN? (3) When do we reach for transfer learning? |

## Section 2 — Why Images Need a Different Model (slides 4–6)

| # | Slide | Purpose |
|---|-------|---------|
| 4 | **An image is just a tensor** | Channels-first convention: (B, C, H, W). Grayscale = 1 channel, RGB = 3. |
| 5 | **The flat-MLP problem** | Side-by-side parameter counts: 28×28 (235K) vs 224×224×3 (38.6M). Linear scaling kills MLPs. |
| 6 | **The spatial-locality problem** | MLPs treat every pixel as independent. They CAN'T exploit locality or translation invariance. We need a model that does. |

## Section 3 — Convolutions (slides 7–11)

| # | Slide | Purpose |
|---|-------|---------|
| 7 | **The kernel intuition** | 3×3 edge-detection kernel: 9 numbers, slides everywhere. ~5,000× fewer params than equivalent MLP layer. |
| 8 | **Multiple kernels = feature maps** | A `Conv2d(out_channels=16)` learns 16 kernels in parallel. Each layer specialises (edges → corners → parts → objects). |
| 9 | **Stride · Padding · Pooling** | The three mechanical operators. Padding=1 preserves size. MaxPool halves it. |
| 10 | **Stacking = compositionality** | Why depth matters: edges → corners → parts → objects. Each layer's effective receptive field grows. |
| 11 | **Code-along checkpoint: NB 02** | "Open `02_convolutions_intuition.ipynb` — apply hand-crafted kernels, verify nn.Conv2d == manual." |

## Section 4 — Building a CNN (slides 12–16)

| # | Slide | Purpose |
|---|-------|---------|
| 12 | **The CNN block** | `Conv → ReLU → MaxPool`. Stack 2-3 of these + a Flatten + Linear classifier head. |
| 13 | **TinyCNN architecture** | Concrete: 105K parameters, 2 conv blocks, beats the MLP on Fashion-MNIST. |
| 14 | **The training loop is unchanged** | The 5-line PyTorch loop from L07. Only the model definition changes. |
| 15 | **CNN vs MLP results table** | Honest: TinyCNN 0.884, FlatMLP 0.871. Small absolute gain but with ~45% of params. Gap widens on harder data. |
| 16 | **Code-along checkpoint: NB 03** | "Open `03_first_cnn.ipynb` — build TinyCNN, train it, visualise learned kernels." |

## Section 5 — Transfer Learning (slides 17–21)

| # | Slide | Purpose |
|---|-------|---------|
| 17 | **Transfer learning — why** | Real catalogues have 500 photos/class, not 60K. Don't train from scratch — adapt an existing model. |
| 18 | **Pretrained models in torchvision** | ResNet18, ResNet50, MobileNetV3, EfficientNet-B0, ViT-B/16. One-line load. |
| 19 | **Two transfer patterns** | Feature extraction (freeze, train head) vs fine-tuning (unfreeze last block at LR 1e-4). |
| 20 | **The honest result** | On Fashion-MNIST: head-only **loses** to from-scratch CNN (domain gap from ImageNet). Fine-tuning recovers. |
| 21 | **Code-along checkpoint: NB 04** | "Open `04_transfer_learning.ipynb` — small subset + ResNet18 + fine-tune." |

## Section 6 — Sarah's Recommendation & Bridge (slides 22–28)

| # | Slide | Purpose |
|---|-------|---------|
| 22 | **Sarah's Friday recommendation** | Pretrained ResNet18, head training, fine-tune layer4 at LR 1e-4. Recipe: 4 steps. |
| 23 | **Production maturity** | Ship the model + a human + a data-collection plan. Day 1 → Month 3 → Month 6 maturity. |
| 24 | **Three things to remember** | (1) Convolutions exploit locality. (2) Training loop unchanged. (3) Don't train CNNs from scratch on small data. |
| 25 | **Method cheat sheet** | When to use what: tabular vs 10K+ images vs <1K/class vs production constraints. |
| 26 | **Common pitfalls** | Normalisation, learning rate, training time, augmentation, domain gap, confusion matrix. |
| 27 | **Bridge to L09** | Marcus's NEXT question: "natural-language search?" → embeddings & NLP (L09 → L10). |
| 28 | **Q&A · Assignment preview** | CIFAR-10 from-scratch CNN + transfer learning, three reflection questions. |

---

## Embedded code-along checkpoints

Slides 11, 16, and 21 are explicit handoffs to the notebooks. Each tells learners which notebook to open and what to expect. Plan ~15-25 minutes per checkpoint.

## Design notes

- **Palette:** Ocean Gradient (NAVY `#065A82`, TEAL `#1C7293`, MIDNIGHT `#21295C`, with ICE `#E8F1F5` for body)
- **Header font:** Trebuchet MS (titles 30pt). **Body font:** Calibri (13–18pt); code in Consolas.
- **Visual motif:** Each conceptual slide has a coloured numerical/iconic label on the left, content block to the right. Consistent across all 28 slides.
- **No AI-tell accent lines** under titles — whitespace and background colour blocks only.

## What's intentionally honest in this deck

- Slide 15 admits the TinyCNN vs FlatMLP gap is small on Fashion-MNIST (~1.5pp). The deck doesn't oversell.
- Slide 20 shows the domain-gap result *with the actual numbers* (0.787 vs 0.715 head-only, fine-tune to 0.832). Students see the real failure mode.
- Slide 23 separates "ship the model" from "ship the system" — including human-in-the-loop and the data-collection plan.

These touches are what make the deck instructor-credible. Don't soften them.
