# Reference — L01 Introduction to Machine Learning

---

## Further Reading and Watching

Use these after completing the lesson. Start with free resources.

### Free — Videos

**StatQuest with Josh Starmer** (YouTube)
Plain-English ML and statistics, one concept per video.
- [Machine Learning Fundamentals — StatQuest playlist](https://www.youtube.com/playlist?list=PLblh5JKOoLUICTaGLRoHQDuF_7q2GfuJF)

**3Blue1Brown — But what is a Neural Network?** *(for when you get to L07, but watch it now for motivation)*
- [Neural Networks chapter 1](https://www.youtube.com/watch?v=aircAruvnKk)

**Fireship — Machine Learning explained in 100 seconds**
A fast overview; good for your non-technical friends.
- [YouTube link](https://www.youtube.com/watch?v=PeMlggyqz0Y)

### Free — Interactive

**Google's Machine Learning Crash Course**
A self-paced, beginner-friendly tour with in-browser code exercises.
- https://developers.google.com/machine-learning/crash-course

**Kaggle Learn — Intro to Machine Learning**
A micro-course (4 hrs) that builds your first model.
- https://www.kaggle.com/learn/intro-to-machine-learning

**Teachable Machine** (by Google)
Train an image/sound classifier in your browser, no code required. Great for non-technical colleagues.
- https://teachablemachine.withgoogle.com/

### Free — Reading

**Machine Learning Yearning** — Andrew Ng
A short, practical guide to structuring ML projects. Free PDF.
- https://www.deeplearning.ai/machine-learning-yearning/

**Rules of Machine Learning: Best Practices for ML Engineering** — Google
What to do and not do on a real ML project. Opinionated and excellent.
- https://developers.google.com/machine-learning/guides/rules-of-ml

### Books (when you want depth)

**Hands-On Machine Learning with Scikit-Learn, Keras and TensorFlow** — Aurélien Géron (2nd/3rd edition)
The best single book for learners who want to go from beginner to competent practitioner. Code-forward, clear explanations.

**The Hundred-Page Machine Learning Book** — Andriy Burkov
Exactly what the title says. Dense but concise. Great as a reference after you've worked through Géron.

**You Look Like a Thing and I Love You** — Janelle Shane
No code. Stories of ML failures and weird behaviours. Excellent for building intuition and laughing at the same time.

---

## Glossary

Key terms from this lesson, in plain English. Terms in **bold** appear in the notebooks.

**Algorithm**
A recipe for solving a problem. In ML, the algorithm turns training data into a model. Examples: linear regression, decision tree, neural network.

**Classification**
A supervised learning task where the label is a category (positive/negative, spam/not-spam, cat/dog/bird). See also: regression.

**Confidence Score**
How sure the model is about a single prediction, usually expressed as a percentage or a number between 0 and 1. A 95% confidence on "POSITIVE" means the model is highly sure; a 55% confidence means it barely picked that label over the other. In practice, teams use confidence to decide which predictions to trust automatically and which to send to a human reviewer.

**Confusion Matrix**
A small 2×2 table that breaks down a classifier's predictions into four buckets: correct-positive, correct-negative, false-positive, and false-negative. Reveals exactly *where* the model fails — not just overall accuracy. Introduced in Part 3 of this lesson and used throughout the course.

**Deep Learning**
A sub-field of ML using neural networks with many layers. Covered in L07 onwards. Often used when classical ML plateaus on complex data like images, text, or audio.

**Deployment**
Putting a trained model where someone or something can use it — in a web service, a mobile app, or a batch pipeline.

**Feature**
An input to the model. For a customer, features could be age, city, number of past purchases. For a photo, features are the pixels (or learned representations of them).

**Feature Engineering**
The art of creating useful features from raw data. Often the highest-leverage part of an ML project. Covered in L04.

**Generative AI (GenAI)**
ML models that produce new content — text, images, audio, code. Covered in L10. Distinguished from *predictive* ML, which outputs a category or number.

**Hyperparameter**
A setting of the learning algorithm that *you* choose (not learned from data) — e.g., how deep a decision tree can grow, or how quickly a neural network learns. Covered in L04.

**Inference**
Using a trained model on new data to get predictions. Opposite of training.

**Label**
The correct answer for a training example. For sentiment analysis, the label is "positive" or "negative." Also called *target* or *ground truth*.

**Machine Learning (ML)**
A discipline that builds systems which improve at a task by learning from data, rather than being explicitly programmed.

**Model**
The learned mapping from features to predictions. Once trained, you use the model over and over for inference.

**Overfitting**
When a model performs well on training data but poorly on new data. The model memorised rather than generalised. Covered in L03.

**Polarity**
A numeric score between -1 (fully negative) and +1 (fully positive) that a sentiment model outputs to describe how negative or positive a piece of text is. TextBlob returns a polarity score for every sentence. Unlike a bare "positive/negative" label, polarity tells you *how strongly* the model believes it.

**Pre-trained model**
A model someone else trained on a large dataset, which you can use directly (or fine-tune). Hugely useful — saves time, compute, and data.

**Precision and Recall**
Two complementary ways to measure a classifier's quality.

- **Precision** = of everything the model labelled positive, what fraction was *actually* positive? (Low precision means lots of false alarms.)
- **Recall** = of everything that was *actually* positive, what fraction did the model catch? (Low recall means missed real cases.)

The right trade-off depends on the cost of each kind of mistake. Covered in depth in L03.

**Regression**
A supervised learning task where the label is a number (house price, temperature, sales). See also: classification.

**Reinforcement Learning**
An ML category where an agent learns by interacting with an environment and receiving rewards. Covered conceptually in this lesson only.

**Supervised Learning**
An ML category where you train on labelled examples. Most business ML is supervised.

**Test set**
A portion of your data held out during training, used only to evaluate the final model. Simulates "performance on unseen data."

**Threshold**
The cut-off score above which a probabilistic or numeric prediction is treated as belonging to one class. A sentiment model might label everything with a polarity above 0 as "positive," but could just as well use 0.2 to be stricter. Most models default to a neutral mid-point, but real businesses rarely do — tuning the threshold is how you translate the business cost of being wrong in one direction versus the other into model behaviour. Covered in depth in L03.

**Training**
The process of an algorithm learning a model from labelled examples.

**Training set**
The data used during training. The model is allowed to see these examples and their labels.

**Unsupervised Learning**
An ML category where training data has no labels; the goal is to discover structure (clusters, patterns, anomalies).

**Validation set**
A third split (separate from training and test), used to tune hyperparameters without peeking at the test set. Introduced in L04.

---

## Cast of Characters — L01

**Sarah Chen** — Customer Experience Analyst, NorthStar Retail. Our protagonist across all 10 lessons. Starts with a business degree, a few SQL and Python evening courses, and no formal ML training. By the end of the course she is leading a small data science team.

**Priya Raman** — CMO at NorthStar and Sarah's manager in L01–L03. The one who hands Sarah the 10,000 reviews on her first Monday. Her recurring pressure — *"Show me the money, not the accuracy"* — will shape how every future model is evaluated.

**Aisha Patel** — Head of Customer Service. She carries the USB drive of reviews into Sarah's office in L01 and becomes a recurring source of real operational data across the course.

**Marcus Wong** — CTO. Appears briefly in L01, more prominently from L04 onward after an early model failure makes him cautious. His trust has to be earned.

**Tom Bradley** — Head of Analytics at **Lakeside Bank**, NorthStar's banking partner. You met him in the Phase 3 assignment. Tom's data is different from Sarah's, but the *approach* is the same — which is the whole point of the Lakeside secondment.

---

## Want to go deeper after this course?

## End-of-module project

The **default project for this module is a Kaggle competition** — you'll pick a contest, apply the L01–L10 workflow end-to-end, and submit your notebook + leaderboard score. Details will be shared in the final lesson.

**Optional alternative:** the **NorthStar 2026 Hackathon** — a team-based project with the Sarah Chen narrative. See [HACKATHON_GUIDE.md](../HACKATHON_GUIDE.md) at the repo root. Pick this only if you'd prefer a team project over the Kaggle contest; it is **not** required.

## Want to go deeper after this course?

The next natural steps are:
- Take a deeper dive with Andrew Ng's [Machine Learning Specialization on Coursera](https://www.coursera.org/specializations/machine-learning-introduction) (free audit available)
- Read *Hands-On Machine Learning* (Géron) from end to end
