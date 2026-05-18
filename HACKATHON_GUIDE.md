# Hackathon Guide

A team-based applied capstone where learners solve a real business problem using the techniques from the course. Runs in both cohorts (3 days for full-time, 1 day for part-time).

---

## Why a Hackathon (and not only Kaggle)

For adult learners without a technical background, the goal is **being able to do something useful at work**, not climbing a leaderboard. A hackathon forces three things Kaggle doesn't:

1. **Problem framing** — you can't start coding until you've defined what "good" means.
2. **Stakeholder communication** — you have to explain your solution to non-technical judges.
3. **End-to-end delivery** — data, model, and presentation in one bundle.

These are the skills learners will actually use when they return to work.

---

## Who runs which hackathon

| Cohort | Duration | Team size | When |
|---|---|---|---|
| Full-time | 3 days (Days 11–13) + 2 days polish & demo (Days 14–15) | 3–4 people | Week 3 of the course |
| Part-time | 1 day (Saturday) | 3–4 people | Weekend 5 of the course |

The full-time version is deeper; the part-time version is a compressed sprint but still goes end-to-end.

---

## Three themes (learners pick one per team)

### Theme A — Business problem track

A realistic, messy dataset from a real domain (retail, healthcare, finance, HR). Example briefs:

- *"Sarah's bakery wants to reduce weekly food waste by 20%. Here are 2 years of sales and weather data. What should she bake more/less of, on which days?"*
- *"This insurance company has 3 years of claims data. Which customers are most likely to file a large claim next year, and why?"*

Success looks like: a working predictive model + a 3-slide explanation a business owner would understand.

### Theme B — GenAI application track

Build a GenAI-powered product MVP using a pre-built LLM (via API). Example briefs:

- *"An HR team spends 4 hrs/week writing performance review templates. Build a GenAI tool that drafts a review from bullet points."*
- *"A retail store wants a chatbot that answers product questions in-store. Build a RAG-based prototype using a small product catalog."*

Success looks like: a runnable demo + discussion of where it would fail in production.

### Theme C — Social impact track

Use a public-interest dataset (Kaggle community datasets, UCI ML repository, or government open data). Example briefs:

- *"Predict which neighborhoods in this city are most underserved by public transit."*
- *"Build a dashboard that flags signals of unusual activity in public spending data."*

Success looks like: a defensible analysis + a recommendation that could be shared with a non-profit or agency.

---

## Judging rubric

Score each team 0–10 on four dimensions. Total weighted score determines ranking.

| Dimension | Weight | What it rewards |
|---|---|---|
| **Business insight** | 40% | Clear problem framing, empathy for the user, non-technical clarity of value |
| **Technical execution** | 30% | Appropriate method choice, working prototype, no hand-waving |
| **Presentation** | 20% | Story arc, demo flow, handling of Q&A |
| **Reflection** | 10% | Honesty about limitations, what they would do with more time |

**Deliberately low weight on technical difficulty.** For non-technical learners, a simple model explained well is better than a complex one poorly justified.

---

## Team roles

Teams of 3–4 split into these loose roles — but everyone should touch the data at some point:

| Role | Responsibilities |
|---|---|
| **Product lead** | Problem framing, user empathy, stakeholder stories, slide narrative |
| **Data / modeling lead** | Cleaning, feature engineering, model choice, evaluation |
| **Prototype / demo lead** | Notebook presentability, demo script, backup plan if demo fails live |
| **Presenter** | Main speaker on demo day (can be shared) |

For part-time 1-day version, combine Product + Presenter roles.

---

## Day-by-day plan (full-time, 3 days + 2 polish days)

### Day 11 — Kickoff & Frame

**Morning (instructor-led):** theme reveal, team formation, judging rubric walkthrough.
**Afternoon (team):** problem framing. By end of day, each team has written:
- A one-sentence problem statement
- The user persona they're building for
- Three "we would consider this successful if..." criteria

### Day 12 — Data & Baseline

**Morning:** data exploration, cleaning.
**Afternoon:** a naive baseline (mean-predictor, rule-based classifier, or off-the-shelf LLM prompt). **Every team must have a runnable baseline by end of Day 12.**

### Day 13 — Iterate

**Morning:** try a second approach (model family, better features, better prompt).
**Afternoon:** pick the better of the two, start integration.

### Day 14 — Polish

**Morning:** presentation deck + demo script.
**Afternoon:** dry-run in front of another team. Incorporate feedback.

### Day 15 — Demo Day

**Morning:** 15 min per team (10 min presentation + 5 min Q&A), judged by instructors and invited guests.
**Afternoon:** **Coaching 3** — individual "how do I take this back to work" conversations.

## One-day plan (part-time, Saturday)

| Time | Activity |
|---|---|
| 09:00 – 09:30 | Theme reveal, team confirmation (teams were formed in Coaching 4) |
| 09:30 – 10:30 | Problem framing: 3 success criteria written down |
| 10:30 – 12:30 | Data exploration + baseline |
| 12:30 – 13:30 | Lunch |
| 13:30 – 15:30 | Iterate on approach |
| 15:30 – 16:30 | Presentation prep |
| 16:30 – 17:30 | Demos (15 min × 3–4 teams) |
| 17:30 – 18:00 | **Coaching 5** — group debrief + take-home plan |

---

## What "good" looks like at demo time

- **Problem is named, user is named.** "Sarah the bakery owner, who throws out ~$2k of bread a week" — not "a business user."
- **Baseline before model.** Teams that say *"our naive baseline predicted X; our model beat it by Y"* score higher than teams that only show the model.
- **Demo actually runs.** Teams practice the demo at least once end-to-end before Day 15.
- **Limitations are named, not hidden.** "This model would need retraining every 3 months because customer behavior shifts." Honest limitations > overclaimed results.

---

## What instructors do during the hackathon

- **Circulate, don't teach.** The course content is done. Help teams *decide*, not *understand*.
- **Watch for stuck teams.** Signs: arguing about scope at hour 3, still no baseline by Day 12, demo not rehearsed.
- **Enforce scope.** Most teams will overreach. The instructor's main job Day 11–12 is cutting scope.
