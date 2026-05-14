# L09 — NLP, Embeddings & Semantic Search · Slide Outline

**Deck:** `L09_slides.pptx` · 28 slides · Ocean Gradient palette
**Total runtime:** ~90 min of slide-driven lecture, interleaved with three code-along checkpoints.

---

## Section 1 — Opening & Recap (slides 1–3)

| # | Slide | Purpose |
|---|-------|---------|
| 1 | **Title** — "NLP & Embeddings" | Set the room. Sarah is six lessons into ML. |
| 2 | **Where we left off** | L08 shipped the auto-tagger. Marcus forwards a customer complaint about search. |
| 3 | **Today's three questions** | (1) Why does keyword search fail? (2) What IS an embedding? (3) How to build production semantic search? |

## Section 2 — Why Keyword Fails (slides 4–5)

| # | Slide | Purpose |
|---|-------|---------|
| 4 | **The failure example** | "blue summer dress" vs "Lightweight floral frock perfect for warm summer days" — one word in common, ten dresses missed. |
| 5 | **Keyword baseline scores** | 5/8 on the 8-query benchmark. Easy queries pass; synonym queries fail. |

## Section 3 — Embeddings (slides 6–10)

| # | Slide | Purpose |
|---|-------|---------|
| 6 | **One-hot vectors** | The naive baseline. Equidistance problem: all distinct words have cosine 0. |
| 7 | **Dense embeddings** | 384-dim, learned, where distance = meaning. Three consequences: compact, meaningful, composable. |
| 8 | **Code-along: NB 02** | Build one-hot, BoW, and toy co-occurrence embeddings by hand. |
| 9 | **Pretrained sentence-transformers** | all-MiniLM-L6-v2: 22M params, 384-dim output, ~80 MB. Two-line API. |
| 10 | **The synonym test** | Concrete cosine numbers: frock↔dress lifts from 0.035 (toy) to 0.249 (pretrained). Honest framing: cosines stay in [0.2, 0.7]; read as ranking not percent. |

## Section 4 — Building the Engine (slides 11–16)

| # | Slide | Purpose |
|---|-------|---------|
| 11 | **Code-along: NB 03** | Load MiniLM, embed catalogue, 2D PCA visualisation. |
| 12 | **The search recipe** | Offline encode + cosine + top-K. Three lines of NumPy. |
| 13 | **Filters on top** | Restrict candidates first (category, price), then rank semantically. Production essential. |
| 14 | **Semantic vs TF-IDF benchmark** | Headline: Semantic 6/8 vs TF-IDF 4/8. Each wins on different query types. |
| 15 | **Production wiring diagram** | Hybrid retrieval (TF-IDF + semantic) → union → filters → re-rank → top-K. |
| 16 | **Code-along: NB 04** | Build SemanticSearch class, TF-IDF baseline, hybrid scoring. |

## Section 5 — Production Reality (slides 17–21)

| # | Slide | Purpose |
|---|-------|---------|
| 17 | **Sarah's Friday recommendation** | Specific recipe + production checklist (re-embed cadence, cold-start, monitoring). |
| 18 | **Embeddings beyond search** | Classification (embed + logistic), clustering (embed + KMeans), similarity (dedup, recs). |
| 19 | **Multilingual embeddings** | paraphrase-multilingual-MiniLM. English query, French/Spanish results. Demoed on dress example. |
| 20 | **Tokenisation up close** | Subword tokens. Common words → 1 token; rare → fragmented. Why SKUs are awkward. |
| 21 | **Vector databases at scale** | NumPy < 10K, FAISS 10K-1M, managed DB 1M+. |

## Section 6 — Recap & Bridge (slides 22–28)

| # | Slide | Purpose |
|---|-------|---------|
| 22 | **Three things to remember** | (1) embeddings = meaning as geometry. (2) Pretrained is the default. (3) Production is hybrid, not pure semantic. |
| 23 | **Method cheat sheet** | When to use what: exact / synonym / hybrid / classification / clustering / multilingual / latency-critical. |
| 24 | **Common pitfalls** | Reading cosine as percent, stale embeddings, skipping filters, wrong model, top-1 trust, noisy input. |
| 25 | **Bridge to L10** | Where do embeddings come from? Attention + transformers. L10 opens the black box. |
| 26 | **Sarah's full journey** | L01 → L09: tools mastered. ML toolkit recap. |
| 27 | **Assignment preview** | Recipe semantic-search engine: Part A semantic, Part B vs TF-IDF, Part C failure-mode reflection. |
| 28 | **Q&A** | Three reflection prompts to take home. |

---

## Embedded code-along checkpoints

Slides 8, 11, and 16 are explicit handoffs to the notebooks. Each tells learners which notebook to open and what to expect. Plan ~20-25 minutes per checkpoint.

## Design notes

- **Palette:** Ocean Gradient (NAVY `#065A82`, TEAL `#1C7293`, MIDNIGHT `#21295C`, with ICE `#E8F1F5` for body)
- **Header font:** Trebuchet MS (titles 30pt). **Body font:** Calibri (12-18pt). Code: Consolas.
- **Visual motif:** Coloured numbered/iconic chips on the left, ICE-coloured content panels on the right.
- **No AI-tell accent lines** under titles — whitespace and background colour blocks only.

## What's intentionally honest in this deck

- Slide 10 admits MiniLM cosines stay in [0.2, 0.7]; *don't* read 0.25 as "25% similar".
- Slide 14 shows the **real** benchmark numbers (semantic 6/8, TF-IDF 4/8) — not aspirational percentages.
- Slide 15 makes hybrid retrieval the headline architecture, not pure semantic.
- Slide 24 calls out six specific pitfalls that learners will actually hit in production.

These touches keep the deck credible. Don't soften them in re-edits.
