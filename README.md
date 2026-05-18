# Machine Learning & Generative AI — A Hands-On Course for Non-Technical Learners

Welcome. This course takes you from *"I've never built a model"* to being able to frame, build, evaluate, and defend practical machine-learning and generative-AI solutions for real business problems.

You do not need a computer science degree. You need curiosity, a laptop, and a willingness to try things.

---

## Who this course is for

- Business analysts, product managers, marketing and operations professionals who want to *use* ML and GenAI in their work
- Team leads who need to evaluate ML projects proposed by others
- Anyone comfortable with spreadsheets who wants to add Python and ML to their toolkit

**Prerequisites:** basic Python and SQL familiarity (an evening course is enough). We teach the ML and GenAI parts from scratch.

---

## How this course works

Each lesson has three parts:

1. **Before class — self-paced, ~75 minutes.** You read a short guide and run one notebook on your own. You come to class already having seen the idea in action.
2. **In class — ~3 hours.** An instructor walks you through deeper notebooks, you work through hands-on exercises, and you discuss with other learners.
3. **After class — self-paced assignment.** You apply what you learned to a fresh business scenario, with sample solutions to check your work.

*This course uses experiential learning: you try, reflect, understand, then apply.* Every lesson follows the same rhythm so you always know what to expect.

**80% hands-on, 20% theory.** You will write and run code in every lesson. The theory is explained in plain English with real-world analogies, not formulas.

---

## The story

Every lesson follows **Sarah Chen**, a Customer Experience Analyst at a mid-sized online retailer called **NorthStar Retail**. Over ten lessons you watch Sarah apply each ML and GenAI technique to a real problem her company is facing. By the end, you will have solved the same problems yourself — and seen where models fail as well as where they succeed.

---

## Lessons

| # | Lesson | Core skill |
|---|---|---|
| L01 | [Introduction to Machine Learning](./L01-intro-ml/) | What ML is, when to use it, the workflow |
| L02 | [Probability & Statistics for ML](./L02-prob-stats/) | Confidence intervals, distributions, A/B testing |
| L03 | [Supervised Learning](./L03-supervised-learning/) | Logistic regression, pipelines, threshold choice |
| L04 | [Supervised Learning — Advanced](./L04-supervised-learning-advanced/) | Trees, ensembles, gradient boosting, tuning |
| L05 | [Unsupervised Learning](./L05-unsupervised-learning/) | PCA, K-Means, anomaly detection |
| L06 | [Time Series Forecasting](./L06-time-series/) | STL decomposition, classical + ML forecasting |
| L07 | [Neural Networks & Deep Learning](./L07-neural-networks/) | MLPs, PyTorch, the training loop |
| L08 | [Computer Vision](./L08-computer-vision/) | CNNs, transfer learning |
| L09 | [NLP & Embeddings](./L09-nlp-embeddings/) | Sentence embeddings, semantic search |
| L10 | [Transformers & GenAI](./L10-transformers-genai/) | Attention, LLMs, RAG |

Lessons are designed to be taken in order.

### Course-wide references

- 📋 [**M3_Course_Roadmap.md**](./M3_Course_Roadmap.md) — the full 10-lesson roadmap, with the Core / Optional split for every lesson. Useful for skimming what's coming.
- 🏆 [**HACKATHON_GUIDE.md**](./HACKATHON_GUIDE.md) — the end-of-course capstone: themes, judging rubric, day-by-day plan for both full-time (3 days) and part-time (1 day) cohorts.

---

## Setup

**Required reading before you run any code:** [**SETUP.md**](./SETUP.md)

The setup guide covers everything you need:

- Installing Python via Miniconda (macOS or Windows WSL)
- Creating the `dsai-m3` conda environment
- Installing VS Code + the Jupyter extension
- When to use Google Colab instead (for L08 and L10, where a GPU helps)
- Troubleshooting the common hang/crash modes

Budget ~15 minutes for first-time setup. Once it's done, you reuse the same environment for every lesson.

### Supported platforms

| Your machine | Setup |
|---|---|
| **macOS** (Intel or Apple Silicon) | VS Code + Jupyter extension + conda env |
| **Windows 10/11** | WSL2 (Ubuntu) + VS Code + Jupyter extension + conda env |
| **No GPU, want to run L08 or L10 faster** | Google Colab — open notebook via the "Open in Colab" badge |

We deliberately do **not** support native Windows Python. PyTorch is far more reliable on Linux (which WSL is) than on Windows.

---

## How to start

1. Read **[SETUP.md](./SETUP.md)** and complete the 15-minute setup
2. Open **[L01-intro-ml](./L01-intro-ml/README.md)** in VS Code
3. Follow the three-part rhythm (pre-class → in-class → assignment) for each lesson

---

## Final project

The course ends with an applied final project where you pick a business problem at NorthStar (or your own organisation) and build a small end-to-end solution using the techniques from L01–L10. Details are released before L08.

---

## For instructors

Each lesson has a `HANDOFF_OPTION_A.md` with:
- A file inventory of what's in the lesson
- Smoke-test results (which notebooks have been run, with what outputs)
- Deviations from the original lesson plan and the reasoning
- Known instructor pitfalls

Start there before teaching a lesson. The `slides_outline.md` in each `slides/` folder is your speaker outline.

---

## Questions or feedback

Open an issue on this repository. We read every one.
