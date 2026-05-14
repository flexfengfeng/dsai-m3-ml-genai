# L10 — Transformers, Attention & GenAI · Slide Outline

**Deck:** `L10_slides.pptx` · 30 slides · Ocean Gradient palette
**Total runtime:** ~90 min of slide-driven lecture, interleaved with three code-along checkpoints.
**This is the closer** — closing slides (25-30) wrap up the entire module.

---

## Section 1 — Opening & Recap (slides 1–3)

| # | Slide | Purpose |
|---|-------|---------|
| 1 | **Title** | "Transformers & GenAI". The final lesson of Module 3. |
| 2 | **Where we left off** | L09 search shipped. Marcus's final ask: a shopping assistant. |
| 3 | **Today's three questions** | (1) What is attention? (2) What is an LLM mechanically? (3) How do you use an LLM on YOUR data? |

## Section 2 — Attention (slides 4–10)

| # | Slide | Purpose |
|---|-------|---------|
| 4 | **The context problem** | "Trophy didn't fit because IT was too big/small" — meaning depends on context. |
| 5 | **Q · K · V — three vectors per token** | Query (looking for), Key (offering), Value (content). The formula. |
| 6 | **The attention formula in plain English** | Three steps: dot products → softmax → weighted sum. |
| 7 | **Multi-head attention** | Several heads, each specialising — coreference, syntax, modifiers. |
| 8 | **The transformer block** | Attention + feed-forward + residuals + LayerNorm. The Lego brick. |
| 9 | **Scaling table** | Same architecture from `all-MiniLM-L6-v2` (22M) to GPT-4 (~1T). Same recipe. |
| 10 | **Code-along: NB 02** | Compute attention by hand. Visualise. Sketch a transformer block in PyTorch. |

## Section 3 — LLMs (slides 11–14)

| # | Slide | Purpose |
|---|-------|---------|
| 11 | **An LLM is a next-token predictor** | Input → probability distribution. Pick one, append, repeat. That's it. |
| 12 | **Sampling parameters** | Greedy / temperature / top-k / top-p. Production defaults. |
| 13 | **Chat templates** | Instruction-tuned models expect SPECIFIC token format. Use `apply_chat_template`. |
| 14 | **Code-along: NB 03** | Load SmolLM2-360M; tokenise; generate; sample; chat-format. |

## Section 4 — RAG (slides 15–20)

| # | Slide | Purpose |
|---|-------|---------|
| 15 | **Hallucination** | LLM alone invents "Sunburst Linen Dress" — fluent, confident, fictional. |
| 16 | **RAG — the fix** | Retrieve top-K from corpus → stuff in prompt → LLM answers with grounding. |
| 17 | **Why RAG works** | LLM brings fluency + reasoning; retrieval brings facts + audit trail. |
| 18 | **Code-along: NB 04** | Build RAGSystem class. See hallucination. See it fixed. |
| 19 | **Sarah's recommendation** | Production guardrails: strict system prompt, validate names, log queries, A/B test. |
| 20 | **Deployment trade-off** | Self-host vs hosted API vs fine-tune. Cost / latency / quality table. |

## Section 5 — Pulling It All Together (slides 21–24)

| # | Slide | Purpose |
|---|-------|---------|
| 21 | **Decision tree** | Prompt → RAG → fine-tune. When to use what. |
| 22 | **Three things to remember** | Attention is the mechanism. LLM = next-token predictor. RAG for YOUR data. |
| 23 | **Method cheat sheet** | When to reach for each tool. |
| 24 | **Common pitfalls** | Hallucination, wrong chat format, wrong temperature, premature fine-tuning. |

## Section 6 — Module 3 Close (slides 25–30)

| # | Slide | Purpose |
|---|-------|---------|
| 25 | **Module 3 toolkit recap** | L01-L10 in a table. Classical ML → DL → NLP → GenAI. |
| 26 | **Sarah's journey — and yours** | What the narrative was teaching about TASTE in tool selection. |
| 27 | **What's next** | Domain specialisation, MLOps, fine-tuning, reading papers. |
| 28 | **Final assignment** | Build a RAG shopping assistant. Three parts. |
| 29 | **Resources** | Books, papers, courses, communities. "Build something every week." |
| 30 | **Closing** | "Thank you. Now go build something." |

---

## Embedded code-along checkpoints

Slides 10, 14, and 18 are explicit handoffs to the notebooks. Each tells learners which notebook to open and what to expect. Plan ~20-25 minutes per checkpoint.

## Design notes

- **Palette:** Ocean Gradient (NAVY `#065A82`, TEAL `#1C7293`, MIDNIGHT `#21295C`, with ICE `#E8F1F5` for body)
- **Header font:** Trebuchet MS (titles 28-30pt). **Body font:** Calibri (12-18pt). Code: Consolas.
- **Closing slide:** full-bleed navy, large "Thank you" + Module 3 sign-off
- **No AI-tell accent lines** under titles — whitespace and background colour blocks only.

## What's intentionally honest in this deck

- Slide 15 shows the hallucination example concretely (invented "Sunburst Linen Dress") — no soft-pedalling
- Slide 20 publishes real cost/latency numbers across self-hosted vs hosted options
- Slide 21 explicitly says "fine-tune is the LAST resort, not the first" — counter to common hype
- Slide 27 says "spend more time shipping than studying" — practical career advice
- Slide 30 ends with the call to action: now go build something

These touches keep the deck credible. Don't soften them.
