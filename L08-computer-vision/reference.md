# L08 Reference · Glossary

| Term | Definition |
|------|------------|
| **Image tensor** | A multi-dim array `(batch, channels, height, width)` representing one or more images. PyTorch convention is "channels-first." |
| **Channel** | One colour or intensity plane of an image. Grayscale = 1 channel, RGB = 3, RGBA = 4. |
| **Pixel** | A single intensity value in an image. Range typically `[0, 255]` raw or `[0.0, 1.0]` after normalisation. |
| **Kernel / Filter** | A small matrix of weights (e.g. 3×3) that's slid over an image to produce a feature map. |
| **Feature map** | The output of one kernel applied across an image — a 2D array showing where the kernel pattern is present. |
| **Convolution** | The operation of sliding a kernel over an input and computing weighted sums. Implemented in PyTorch as `nn.Conv2d`. |
| **Stride** | The step size (in pixels) between successive kernel applications. Stride 2 produces an output ~half the input size. |
| **Padding** | Extra pixels (usually zeros) added around the input so kernels can apply near the edges. `padding=1` for a 3×3 kernel preserves input size. |
| **Pooling** | A downsampling operation. **Max pooling** takes the max over a small window (commonly 2×2). Used to shrink feature maps and add translation invariance. |
| **Locality** | The property that nearby pixels are more related than far-apart ones. Convolutions exploit this; MLPs ignore it. |
| **Translation invariance** | A feature detector should fire whether the feature is in the top-left or bottom-right of the image. Convolutions get this for free. |
| **Receptive field** | The region of the input image that a particular output neuron "sees." Grows as you stack conv layers. |
| **CNN (ConvNet)** | A neural network whose feature-extracting layers are convolutional. Almost universal for image tasks pre-2020. |
| **Backbone** | The convolutional feature-extracting trunk of a CNN. Usually loaded pretrained; the **head** on top is trained for the new task. |
| **Head** | The final classification layer(s) sitting on top of the backbone. Replaced when adapting a pretrained model to a new task. |
| **Transfer learning** | Starting from a model trained on a large dataset (e.g. ImageNet) and adapting it to your task. Standard practice for image work. |
| **Fine-tuning** | A specific transfer-learning pattern: unfreeze part or all of the pretrained backbone and train it (usually at a small learning rate) on your data. |
| **Feature extraction** | The other transfer-learning pattern: freeze the backbone entirely, train only the head. Cheaper, smaller learning curve. |
| **Data augmentation** | Random transformations applied to training images (flip, crop, rotate, colour jitter) to multiply effective dataset size. |
| **BatchNorm** | A layer (`nn.BatchNorm2d`) that normalises activations per batch. Speeds up training and stabilises gradients. Common after conv layers. |
| **Dropout** | A regularisation layer that randomly zeros activations during training. Reduces over-fitting. |
| **ImageNet** | The 1.2M-image, 1,000-category dataset used to pretrain most CV backbones. Pretrained models come from ImageNet by default. |
| **ResNet** | A family of CNN architectures (He et al., 2015) that introduced **residual connections**, enabling much deeper networks. `ResNet18` and `ResNet50` are the workhorses. |

---

## Further reading & watching

Optional resources that go deeper than what we cover in class. Pick what interests you; nothing here is required.

### Videos

- [But what is a Convolution?](https://www.youtube.com/watch?v=KuXjwB4LzSA) — 3Blue1Brown, ~25 min — *The single operation that makes CNNs work, visually*
- [But what is a Neural Network?](https://www.youtube.com/watch?v=aircAruvnKk) — 3Blue1Brown — first 8 min — *Re-skim from L07 with image classification in mind*
