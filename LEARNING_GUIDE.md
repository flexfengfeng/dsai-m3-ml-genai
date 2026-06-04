# Learning Guide: How to Study with This Repository

Welcome! This repository is designed around **experiential learning**: you try, reflect, understand, and then apply. Instead of starting with dry mathematical formulas, you will see machine learning work (and fail) in code first, and then build the concepts around it.

This guide explains how to navigate each lesson in this repository, using **Lesson 01 (L01 — Introduction to Machine Learning)** as a concrete template.

---

## 📅 The 3-Phase Weekly Rhythm

Every lesson in the course folder (from `L01` to `L10`) is structured into a 3-phase cycle. Here is how to tackle them using **L01** as your guide:

```
  Phase 1: Pre-Class (~25 minutes)       Phase 2: In-Class (~3 hours)        Phase 3: Post-Class (Self-Paced)
  [pre-class.md]                         [lesson.md + slides]                [lesson.md Self-Check]
        |                                       |                                     |
        v                                       v                                     v
  Run 01_monday_morning.ipynb           Run Code-Alongs (02, 03, 04)          Complete assignment.ipynb
  (Observe rules vs. ML)                (Use "Pause & Predict")               (Lakeside Bank Scenario)
        |                                                                             |
        v                                                                             v
  Complete reflection questions                                             Optional: optional_extensions.ipynb
```

---

## 1. Phase 1 — Before Class: Experience First (~25 min)

The goal of this phase is to experience the "magic" of a model before you understand how it works, leaving you with questions to bring to class.

*   **Step 1:** Open [L01-intro-ml/pre-class.md](./L01-intro-ml/pre-class.md). It outlines what you need to do.
*   **Step 2:** Open and run [L01-intro-ml/notebooks/01_monday_morning.ipynb](./L01-intro-ml/notebooks/01_monday_morning.ipynb) in VS Code. 
    *   **Action:** Select the `dsai-m3` kernel in the top right. Run every cell top-to-bottom. You will see a traditional rule-based program fail to classify retail customer reviews, followed by a pre-trained ML model succeeding in seconds.
*   **Step 3:** Answer the reflection questions at the bottom of the pre-class guide. Hold your answers in your head or scribble them down—just make sure you have thought about *why* the rules failed.

---

## 2. Phase 2 — In Class: Deepen & Practice (~3 hours)

During the class session, the instructor will combine slide-driven concepts with guided code-along notebooks.

*   **Step 1: The Slides.** The slides outline core theory with speaker notes (located in `./L01-intro-ml/slides/`).
*   **Step 2: Just-in-Time Reading.** Keep [L01-intro-ml/lesson.md](./L01-intro-ml/lesson.md) open. It is your distraction-free "concept reference" containing clear, plain-English definitions of terms like *features, labels, models, and training*. Refer to it during notebook transitions.
*   **Step 3: The Code-Alongs.** Open and run the in-class notebooks in order:
    1.  [02_what_is_ml.ipynb](./L01-intro-ml/notebooks/02_what_is_ml.ipynb) — Compare rule-based vs. modern DistilBERT models.
    2.  [03_three_categories.ipynb](./L01-intro-ml/notebooks/03_three_categories.ipynb) — Learn Supervised vs. Unsupervised vs. Reinforcement Learning.
    3.  [04_ml_workflow.ipynb](./L01-intro-ml/notebooks/04_ml_workflow.ipynb) — Practice the 7-step machine learning workflow (frame, collect, clean, train, evaluate, deploy, monitor).

> **Important Boundary:** Every notebook contains **Pause & Predict** prompts. Do not just hit `Shift + Enter` without stopping. Stop, read the prompts, and make a prediction. Active prediction is the key to training your brain's intuitive pattern recognition.

---

## 3. Phase 3 — After Class: Independent Application (Self-Paced)

This is where you transfer the skills you learned during class to a brand-new scenario.

*   **Step 1: Self-Check.** Go to the bottom of [lesson.md](./L01-intro-ml/lesson.md) and complete the **Check your understanding** scenarios. Try answering without looking at the sample solutions first.
*   **Step 2: The Assignment.** Open [L01-intro-ml/notebooks/assignment.ipynb](./L01-intro-ml/notebooks/assignment.ipynb).
    *   **Choose your track:** Choose the **🟢 Foundational Track** if you are new to programming and ML. Choose the **🔵 Advanced Track** if you have prior programming background and want to build the code with less hand-holding.
    *   **The Scenario:** You leave NorthStar Retail and go on secondment to **Lakeside Bank**, where you help Tom Bradley categorize 8,000 banking complaints using the workflow you just practiced.
*   **Step 3: Double Check.** Compare your answers to the **Sample Solutions** cell at the bottom of the assignment notebook.

---

## 🚀 Going Deeper (Optional Extensions)

If you have completed the assignment and want to look under the hood:
1.  **Run the optional notebooks:** In L01, open [notebooks/optional_extensions.ipynb](./L01-intro-ml/notebooks/optional_extensions.ipynb) to study polynomial features, the K-Nearest Neighbors algorithm, a perceptron neural network from scratch using NumPy, and train/test split leakage traps.
2.  **Read the reference guide:** Open [reference.md](./L01-intro-ml/reference.md) to explore curated videos, recommended readings, and a glossary of terms.

---

## 🛠️ Key Tips for Success in This Repo

*   **Cap your threads on macOS:** If your Hugging Face sentiment code hangs on macOS, make sure you open VS Code at the repository root folder (`code .`), which allows the `.env` settings to automatically limit threads.
*   **Trust the cache:** The first time you load a model via `pipeline("sentiment-analysis")`, it downloads a ~268 MB weights file. Don't panic if it takes 1-2 minutes; it is cached locally and subsequent executions are instant.
*   **Avoid the "Syntax-Only" Trap:** Make sure you can explain *what decision* a model's prediction is helping a business make. A high-accuracy model that solves the wrong problem is worth zero!
