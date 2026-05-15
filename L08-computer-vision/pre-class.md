# L08 Pre-class · ≈ 75 minutes

> **Goal:** Walk into class already convinced that images need a different model than tabular data, and ready to follow along with convolutional operations.

## Step 1 — Watch the convolution video (≈ 25 min)

3Blue1Brown, *"But what is a convolution?"*  
https://www.youtube.com/watch?v=KuXjwB4LzSA

Focus questions to keep in mind:
- What does a single kernel actually compute?
- Why does the output get smaller? (Stride, padding)
- When the video shows the moving-average kernel — what would happen if you used a different one?

## Step 2 — Optional watch: how CNNs see (≈ 15 min)

3Blue1Brown, *"But what is a neural network?"* — first 8 minutes.  
You've seen this in L07; re-skim with image classification in mind.

## Step 3 — Run [notebooks/01_monday_morning.ipynb](notebooks/01_monday_morning.ipynb) (≈ 25 min)

Sarah opens her laptop on Monday morning with one task: *load the Fashion-MNIST dataset and try the simplest thing she already knows — an MLP.*

The notebook walks through:
1. Loading Fashion-MNIST (10 classes of 28×28 grayscale apparel images)
2. Visualising a few examples per class
3. Flattening every image to a 784-dim vector
4. Training a 2-layer MLP and recording accuracy and parameter count

Expected outcome: ~87% test accuracy with **~235K parameters**. Good enough to ship? Sarah is not sure. Marcus said "ten thousand new photos a season" — 13% mis-tag rate means ~1,300 wrong tags a season.

## Step 4 — Three thought-questions (≈ 10 min)

Write your answers in a notebook cell or scratch pad. We'll discuss in class.

1. The MLP has ~250K parameters for 28×28 images. If product photos were 224×224 (a common real size), how many parameters would the **first layer alone** need for a hidden size of 256? Does that scale?
2. The MLP treats the pixel at `(0,0)` as completely independent from `(0,1)`. Why is that a bad assumption for images?
3. Look at two of the Fashion-MNIST images side-by-side — one shirt, one coat. What would you *want* a model to look at to tell them apart? Sleeves? Collar? Edges?

---

**You are ready for class when:** you've finished the notebook, you can explain why an MLP on raw pixels is inefficient, and you have at least one guess for what's going to fix it.
