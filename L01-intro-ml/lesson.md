# Lesson — L01 Introduction to Machine Learning

> **Chapter 1 of the NorthStar Retail story.** *Sarah Chen · Customer Experience Analyst · January 2023.*
> Early in her second week, Aisha from Customer Service hands her a USB drive with 10,000 reviews. Priya, her manager, wants a sentiment breakdown by Friday.

Use this document as your concept reference — before, during, and after the session. Each section explains a key idea in plain English, anchors it to Sarah's scenario at NorthStar, and shows why it matters for the rest of the course.

| Section | Notebook | Time |
|---|---|---|
| Part 1: What is ML? | `notebooks/02_what_is_ml.ipynb` | ~60 min |
| Part 2: Three categories of ML | `notebooks/03_three_categories.ipynb` | ~45 min |
| Part 3: The ML workflow (applied to Sarah's project) | `notebooks/04_ml_workflow.ipynb` | ~45 min |
| Check your understanding | at the end of this document | ~15 min |

---

## Where does Machine Learning fit? — a bridge from M1 and M2

If you are coming to M3 from the earlier modules, it is worth being explicit about what is new here. The three modules of the programme do different things with the same underlying data:

| Module | What you do with data | One-line description |
|---|---|---|
| **M1 — Data Analytics** | *Summarise the past.* | Averages, trends, dashboards, reports. "What happened last quarter?" |
| **M2 — Data Engineering** | *Move and store data.* | Pipelines, warehouses, clean tables. "Get the data to the right place in the right shape." |
| **M3 — Machine Learning** | *Predict the unseen.* | Given the past, what will likely happen? Or: what is this new thing I've never seen before? |

Sarah's situation is a good illustration. The reviews Aisha handed her already exist (M2 got them into a CSV). Sarah could compute the average review length and report it (M1). But *classifying each review as positive or negative* — especially when some are sarcastic, mixed, or use unusual wording — is a task where no one can write down the rules. That's an M3 problem.

**A useful mental test:** if the answer to the question requires a judgement about a *new* thing you haven't summarised before, you are probably in ML territory.

---

## Why Machine Learning matters

Every business generates data — customer interactions, sales, support tickets, sensor readings, photos, documents. Traditional software turns that data into decisions by following rules a programmer writes. Machine Learning turns that data into decisions by **letting the computer discover the rules itself** from examples.

This matters for three reasons:

1. **Some problems have no clean rules.** Nobody can write a rule that defines a "cat" pixel-by-pixel or a "positive review" word-by-word, but millions of labelled examples exist.
2. **Data is cheaper than expertise.** A company often has more data than it has experts to hand-write rules for.
3. **Patterns change over time.** A rule-based fraud detector is outdated the moment fraudsters change tactics; an ML model retrained on new data keeps up.

You will see all three reasons play out in Sarah's scenario at NorthStar Retail.

---

## Part 1: What is Machine Learning?

### The core idea — two kinds of programming

**The idea in plain English:** In traditional programming, a person writes rules and the computer follows them. In Machine Learning, a person provides examples (inputs and correct outputs) and the computer figures out the rules by itself.

**Real-world analogy:** Imagine teaching a child the difference between a dog and a cat. You could try to write a rulebook: "dogs have floppy ears, cats have pointy ears, dogs bark..." You'd fail — there are too many exceptions. Instead, you show the child many dogs and many cats, each labelled, and their brain builds its own sense of what's what. That's Machine Learning.

**Why it matters:** Every single method we study in this course (regression, trees, neural networks, GenAI) is a different way of "showing examples to the computer and letting it learn the rules." Understanding this frame keeps the course from feeling like a pile of unrelated tricks.

---

### Key vocabulary

| Term | Plain-English meaning |
|---|---|
| **Features** | The *inputs* to the model — whatever you know about a situation. For a customer review, features are the words; for a loan application, features are age, income, employment length. |
| **Label** | The *answer* — what we want the model to predict. For reviews, labels are "positive" / "negative"; for loans, "defaulted" / "repaid." |
| **Model** | The learned "rules" that map features to a prediction. You can think of it as a function: `prediction = model(features)`. |
| **Training** | The process of showing labelled examples to an algorithm so it builds the model. |
| **Inference** | Using a trained model on new, unlabelled data. (Sarah runs inference when she applies the model to her 10,000 unclassified reviews.) |
| **Pre-trained model** | A model someone else trained on a huge dataset. You can use it directly without doing the training yourself. This is what Sarah uses in class. |

---

### When is ML the right tool?

ML is the right tool when **all three** of these are true:

1. **The rules are hard to write by hand** (not a formula, not a policy you can encode).
2. **You have data with examples** of inputs and correct outputs — or you can get it.
3. **Being right most of the time is good enough** — ML is probabilistic, not deterministic.

ML is the **wrong** tool when:

- The rules are obvious and exact (tax calculations, unit conversions).
- The cost of any wrong answer is catastrophic (flight-control software).
- You have no data. ML without data is astrology with more maths.

---

### A first look at data before modelling — descriptive stats in plain English

Before training any model, practitioners spend time *looking at the data*. Even if you never touch a formula, three ideas show up in every ML project and are worth meeting now — informally. We will come back to them with proper maths in **L02 (Probability & Statistics for ML)**.

**Distribution — the shape of the data.** A *distribution* is just the pattern of how often different values appear. If Sarah plots the length of all 10,000 reviews, she might see most reviews are 20–80 words, with a long thin tail of very long complaints. The shape tells her what "typical" looks like, and where the outliers are.

**Mean and spread — where the middle is and how far things stray.** The *mean* is the average (add them up and divide). The *spread* (called *standard deviation* when we get formal in L02) is a one-number answer to "how far from the average do values usually sit?" Together, mean and spread summarise a column of data in two numbers — the mental shorthand behind phrases like "average review is 40 words, give or take 15."

**Correlation — do two things move together?** If longer reviews tend to be more negative, we say review length and sentiment are *correlated*. Correlation is not causation — but noticing it is often where a useful project idea starts.

For L01, that's all the stats you need: know what these three mean in English. L02 adds the formulas, the Central Limit Theorem, confidence intervals, and how to tell whether a correlation is real or noise.

---

## Part 2: The three categories of ML

Every ML problem fits into one of three broad categories — distinguished by **what data you have** and **what you want the model to do**. In L01 we introduce all three verbally so you can recognise them, but **we only run supervised learning hands-on this week** (Sarah's sentiment task). Later lessons go deeper on each.

### Supervised Learning

**The idea in plain English:** You have input-output pairs (features + label), and you want the model to learn the mapping so it can predict labels for new, unlabelled inputs.

**Real-world analogy:** A student studying past exam papers with answers. They see many questions and the correct answers, and over time, they learn to answer new questions on their own.

**Sarah's example:** her sentiment task is supervised. The model learned from many past reviews labelled positive or negative; now it can predict the label for any new review. **This is what you ran in Part 1.**

**Why it matters:** Supervised learning is **the workhorse of business ML**. Churn prediction, fraud detection, demand forecasting, sentiment analysis, loan approval — all supervised. Lessons **L03 and L04** cover this in depth.

**When to use:** You have labels. You want predictions.

---

### Unsupervised Learning

**The idea in plain English:** You have input data but no labels. You want the model to find **structure** in the data — groups, patterns, anomalies — on its own.

**Real-world analogy:** A librarian faced with a crate of new books, none labelled by genre. She groups them by "these feel similar" — romance over here, mystery over there — even though no one told her what the genres are.

**Sarah's example:** if NorthStar wanted to discover *what kinds of customers* it has — people who buy formal wear only, people who buy activewear on weekends, people who return everything — without anyone labelling them in advance, that's unsupervised. We will do exactly this in **L05 (Unsupervised Learning)**.

**When to use:** You don't have labels — or getting labels is too expensive — and you want to understand what's in the data.

---

### Reinforcement Learning

**The idea in plain English:** An agent learns by interacting with an environment: it tries actions, gets rewards or penalties, and adjusts its behaviour over time to maximise reward.

**Real-world analogy:** Training a dog with treats. Sit → treat. Bark at the mailman → no treat. The dog learns which actions pay off.

**Sarah's example:** NorthStar doesn't really have an RL problem in L01's story, but a plausible one would be a recommendation system that learns over time which product suggestions turn into purchases — the reward is the purchase, the action is the suggestion.

**Why it matters:** Reinforcement learning is the engine behind game-playing AIs, robotic control, ad bidding, and parts of how modern LLMs are trained. **We do not build RL systems in this course** — it is a large field beyond the 10-lesson scope — but you should recognise the category when it appears in the wild.

**When to use:** You can't list correct answers in advance — you can only give the system *feedback on what it does*, and you need it to explore.

---

### Comparison at a glance

| Dimension | Supervised | Unsupervised | Reinforcement |
|---|---|---|---|
| **Data you need** | Features + labels | Features only | Environment + reward signal |
| **Goal** | Predict labels | Find structure | Maximise reward over time |
| **Example from Sarah's world** | Classify a review as positive or negative | Discover hidden customer groups | Recommendation system that learns from purchases |
| **Covered in this course** | L01, L03, L04, L08, L09, L10 | L05 | Mentioned, not built |

---

## Part 3: The ML workflow (applied to Sarah's project)

An ML project is not "train a model." It's a process with seven steps, and most of the work is **before and after** training. Rather than run a new experiment, we apply this workflow retrospectively to the sentiment project Sarah has already completed — it fixes each step to one concrete thing.

### The 7 steps

1. **Frame the problem.** What is the business question? What will someone *do* with the answer? What counts as success?
2. **Collect the data.** Where does it live? Is it enough? Is it representative?
3. **Clean and explore.** Missing values, duplicates, wrong types, biased samples.
4. **Train the model.** Choose a method, fit it to the data.
5. **Evaluate on unseen data.** Is it good enough on examples it hasn't seen?
6. **Deploy.** Make the model available to the people or systems that will use it.
7. **Monitor.** The world changes; the model decays. Track its performance over time.

### Common time split

On a real project, the time distribution often looks like:

- Framing + collection + cleaning: **60–70%**
- Training + evaluation: **10–20%**
- Deployment + monitoring: **20%**

**Beginners often imagine the split is the opposite.** Understanding this split is part of becoming a practitioner.

---

### Framing: the most important step

**The idea in plain English:** Before you write any code, get specific about:
- What is the **question**? ("Will this customer cancel?")
- Who will **act** on the answer? (Retention team, marketing automation, nobody?)
- What is **success**? (90% accuracy? Saving $100k in churn? Customer delight?)
- What is the **cost of being wrong**? (Both kinds — false positives and false negatives.)

**Real-world analogy:** Before a detective starts searching a house, they ask: *"What am I actually looking for? Who will use the evidence? What happens if I find nothing?"* ML projects without framing are detectives searching without a theory.

**Why it matters:** A beautifully trained model that solves the wrong problem is worth zero. Sarah's task seems obvious ("classify reviews") until you ask: *positive/negative only? or also topic? whose definition of "positive"? and what happens after she labels them?*

---

### Evaluation: keeping yourself honest

**The idea in plain English:** A model that scores 99% on the data it was trained on might score 60% on data it has never seen. You must hold out some data as a test set and only score the model on that.

**Real-world analogy:** A student who memorised the answers to last year's exam might score 100% on that exam — but tell you nothing about whether they understand the material. The real test is a new exam.

**Why it matters:** Every model evaluation in this course — and every serious ML project in the world — uses a train/test split (or better, cross-validation, which we will cover in **L03**).

---

### Deployment and monitoring — the hidden 40%

**The idea in plain English:** Once the model works in a notebook, you have to (a) make it runnable in production (API, batch pipeline, embedded), (b) track how it is doing in the wild, and (c) decide when to retrain.

**Real-world analogy:** Shipping a product is different from designing a prototype. A prototype that works on the lab bench may fall apart when actual customers use it. Same for models.

**Why it matters:** Many beginner projects stop at Step 5 (evaluation on a notebook). Senior practitioners are distinguished by how seriously they take Steps 6 and 7.

---

## Putting it all together

A real ML practitioner's day is not "training models." It is **choosing** whether ML is the right tool in the first place, **framing** the problem to ensure the model will be used, **preparing** the data (the bulk of the work), and **monitoring** what happens once the model is live.

You will see this pattern repeat through every lesson in this course. We will go deeper on probability and statistics in L02, supervised methods in L03–L04, unsupervised in L05, time series in L06, neural networks in L07, vision in L08, language in L09, and transformers + practical GenAI in L10 — but every lesson sits inside this same outer workflow.

---

## Sarah's unanswered question (the bridge to L02)

By the end of class, Sarah has classified 10,000 reviews in minutes. But Priya is quiet for a moment and then asks:

> *"Sarah — your model says most reviews are positive. But are the positive ones actually positive? How do we know we can trust that number?"*

Sarah doesn't have an answer today. That question — *how sure are we?* — is the engine of **L02 (Probability and Statistics for ML)**. Come to L02 ready to help her answer it.

---

## Check your understanding

Work through these after you have finished the three Part notebooks. Try each question on your own first — the sample answer follows. If a question feels unclear, revisit the relevant section of this document.

---

### Part 1 — What is ML?

**Q1 — Rule-based or ML?** NorthStar wants to automatically send a coupon to every customer whose birthday is today. Which approach fits?

> **Sample answer:** Rule-based. "Is today equal to their birthday?" is a one-line `if` statement. No data needed, no uncertainty. An ML model here would add cost and risk for zero benefit.

**Q2 — Fraud detection fit.** A bank has millions of historical transactions labelled fraudulent / not-fraudulent. Which of the three "ML is the right tool" requirements do they satisfy, and which one might be tricky?

> **Sample answer:** They satisfy #1 (fraud patterns are too varied to hand-code) and #2 (lots of labelled data). Requirement #3 is tricky: a false negative (missing real fraud) is expensive but tolerable; a false positive (blocking a legitimate customer) is annoying but recoverable. They need to weigh the cost of each kind of wrong answer — a classic ML design decision we revisit in **L03**.

**Q3 — Training vs inference.** What's the difference between training and inference?

> **Sample answer:** Training is the (often slow, compute-heavy) process of learning the rules from labelled examples. Inference is using the trained model to predict on new data — it is what you do every time the model gives you an answer. In Sarah's case, someone else already trained the sentiment model; she only does inference.

**Q4 — M1 / M2 / M3 test.** Aisha asks Sarah to (a) count how many reviews came in each month of last year, (b) merge the review table with the orders table in the warehouse, and (c) decide whether a new incoming review mentions a shipping complaint. Which module does each task belong to?

> **Sample answer:** (a) M1 — summarising the past. (b) M2 — moving/joining data. (c) M3 — judgement about a new thing, no simple rule. Only (c) needs Machine Learning.

---

### Part 2 — Three categories

**Q5 — Sarah's review problem.** Which category does Sarah's review-classification problem fit into, given she has a training set of 50,000 past reviews already labelled positive or negative?

> **Sample answer:** Supervised learning. She has features (the review text) and labels (positive / negative). She wants the model to predict labels for new reviews.

**Q6 — Newspaper topics.** A newspaper wants to automatically group its last 10 years of articles into "topics" — without specifying the topics in advance. Which category?

> **Sample answer:** Unsupervised. No labels. The goal is to discover structure (what topics exist). This is the kind of problem we tackle in **L05**.

**Q7 — Same data, different goal.** Could Sarah's review problem also be tackled with *unsupervised* learning? What would be different?

> **Sample answer:** Yes — she could cluster the 10,000 reviews into, say, 5 groups based on how similar they are. She would not know which group is "positive" or "about sizing" without inspecting a sample. It would be faster to set up (no labels needed) but less useful to Priya, who asked specific labelled questions.

---

### Part 3 — Workflow

**Q8 — Framing check.** Priya says "90% accuracy is fine." Is the framing complete?

> **Sample answer:** No. Open questions:
> - *Accuracy on what?* Overall, or on the rare "severe complaint" cases specifically?
> - *Who acts on it?* If an angry-customer coupon is auto-sent, a false positive costs real money.
> - *What about topic tagging?* Priya also wanted sizing / quality / delivery — a separate problem.
> Push back is part of the job. Reframing the question is the work.

**Q9 — Train vs test score.** A model scored 98% on the training set and 70% on the held-out test set. What's going on, and what would you do?

> **Sample answer:** Classic **overfitting** — the model memorised the training data instead of learning general patterns. You would try a simpler model, more regularisation, more training data, or fewer / better features. We cover overfitting properly in **L03 and L04**.

**Q10 — Monitoring.** You deploy a fraud detection model. Six months later accuracy has dropped 10%. What happened, and which step of the workflow failed?

> **Sample answer:** Likely **data drift** — fraudsters changed tactics, or normal customer behaviour shifted. Step 7 (monitoring) caught the decay; Step 4 (training) or the data pipeline needs to be rerun on more recent data. This is why "train once and forget" does not work in practice.

---

**Before you move on,** work through the three Part notebooks, then attempt the assignment (Sarah goes on secondment to Lakeside Bank).
