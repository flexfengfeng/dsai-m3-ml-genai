# L10 — Transformers, Attention & GenAI · Option A Handoff

**Status:** ✅ Complete — **MODULE 3 COMPLETE**
**Build date:** 2026-05-14
**Pattern:** Option A — slide-driven lecture (~90 min) + 4 in-class notebooks (~90 min), 🟡 Extension boundary separating Core from self-study.

---

## What's inside

```
L10-transformers-genai/
├── README.md                            Sarah's brief + Phase 1/2/3 structure
├── setup.md                             Environment + model-download notes
├── pre-class.md                         75-min self-study (3Blue1Brown attention + NB01)
├── lesson.md                            Concept reference + Sarah's recommendation
├── reference.md                         24-term transformer/GenAI glossary
├── environment.yml                      Conda env spec
├── slides/
│   ├── L10_slides.pptx                  30 slides, Ocean Gradient palette
│   └── slides_outline.md                Slide-by-slide breakdown
├── notebooks/
│   ├── 01_monday_morning.ipynb          Pre-class — HuggingFace pipelines hook
│   ├── 02_attention_intuition.ipynb     Attention by hand + visualisations
│   ├── 03_using_an_llm.ipynb            Tokenisation + generation + chat templates
│   ├── 04_rag_pipeline.ipynb            RAG end-to-end with SmolLM2 + MiniLM
│   ├── assignment.ipynb                 Build a RAG shopping assistant
│   ├── optional_extensions.ipynb        Prompt engineering / sampling / when to fine-tune
│   └── data/
│       └── northstar_catalogue.csv      Copied from L09 (76 products)
└── HANDOFF_OPTION_A.md                  (this file)
```

## Smoke-test results

All 6 notebooks executed end-to-end with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1`:

| Notebook | Code cells | Status | Key result |
|----------|-----------:|--------|-----------|
| 01_monday_morning            |  5 | ✅ | Pipelines: sentiment, NER, zero-shot classification — all working; sentiment misclassifies a return as POSITIVE (honest failure mode) |
| 02_attention_intuition       | 11 | ✅ | Attention computed by hand; multi-head visualisation; TransformerBlock built in PyTorch |
| 03_using_an_llm              | 11 | ✅ | SmolLM2-360M loaded; tokenization shown; manual + library generation; sampling sweep; chat templates |
| 04_rag_pipeline              | 12 | ✅ | LLM hallucinates without context; RAG grounds it; RAGSystem class with 4 example queries |
| assignment                   |  4 | ✅ | 5-query benchmark runs cleanly; grading template + reflection structure |
| optional_extensions          |  5 | ✅ | Zero-shot vs few-shot vs role-conditioned; sampling parameter sweep; decision tree |

## Narrative beats

1. **L09 → L10 bridge:** Marcus's question — *"Can we build a shopping assistant?"* — drives the entire lesson.
2. **Pipelines hook (NB01):** Three one-line pipelines feel like magic. Then the sentiment classifier mislabels a return as POSITIVE — a real failure mode, called out honestly.
3. **Attention demystified (NB02):** Four matrix ops produce the entire mechanism behind every modern NLP model. Heatmaps show what attention "looks at".
4. **LLM mechanics (NB03):** SmolLM2-360M loaded; tokenisation shown end-to-end; manual generation loop in 10 lines; sampling parameters compared; chat templates demonstrated.
5. **RAG is the punchline (NB04):** LLM alone invents fake products. With retrieval, it produces grounded answers from real catalogue. The architecture that powers every "chat with your data" product.
6. **Sarah's final recommendation:** RAG with hosted API in production, SmolLM2 for the demo; strict system prompt; validate names against catalogue; A/B test before broad rollout.
7. **Module 3 close:** L10 doubles as the module wrap-up. Slides 25-30 recap the entire 9-lesson toolkit and point to next steps.

## Deviations & notes for the instructor

### 1. Model choice: SmolLM2-360M (361M params, ~720 MB)

This is a **small** instruction-tuned LLM. On CPU it generates 5-15 tokens/sec — slow but usable for the lesson. On GPU it's effectively instant.

**Quality caveat:** at 361M parameters, SmolLM2 is much weaker than Claude/GPT-4. Some responses in the notebooks are slightly off or could be sharper. The narrative explicitly says "for production, swap to a hosted API for quality". The lesson is about the MECHANICS of LLMs and RAG; learners shouldn't expect ChatGPT-grade output from a 360M model on CPU.

If the instructor has a GPU available, swapping to `Qwen/Qwen2.5-1.5B-Instruct` or `microsoft/Phi-3-mini-4k-instruct` will dramatically improve answer quality. See the comment in NB03 cell 1 for swap instructions.

### 2. Honest failure modes called out

The lesson explicitly flags several places where the small model misbehaves:

- **NB01:** sentiment classifier labels a customer return ("returned the colour was MUCH brighter") as POSITIVE — used as a teaching moment about off-the-shelf models being weakest on out-of-distribution patterns
- **NB04:** before RAG, the LLM hallucinates "Sunburst Linen Dress" — a fictional product. The notebook explicitly says THIS DOES NOT EXIST and frames it as the central problem RAG solves
- **Extensions:** the prompt-only category classifier gets some products right, some wrong — used to motivate the "prompt vs fine-tune" decision

These aren't bugs; they're the points the lesson is making. Don't soften them.

### 3. `transformers` library version sensitivity

Built and tested against `transformers==5.8.1`. The newer 5.x line removed some pipeline tasks that 4.x supported (e.g., `summarization`, `question-answering` as default pipelines). NB01 uses **`zero-shot-classification`** instead — a much more useful and impressive demo anyway.

If a learner is on `transformers<5`, the notebook will still work; some pipeline names are slightly different but the code paths used here are stable across versions.

### 4. PyTorch threading

Same as L07-L09. All smoke tests run with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1` and the notebooks call `torch.set_num_threads(1)` at the top. Without this, kernel crashes can occur during long generation runs on macOS.

### 5. Disk usage from cached models

After running all notebooks, `~/.cache/huggingface/hub/` will contain:

- `all-MiniLM-L6-v2` (~80 MB, from L09)
- `distilbert-sst-2` (~268 MB)
- `bert-base-NER` (~436 MB)
- `bart-large-mnli` (~1.6 GB — the zero-shot model)
- `SmolLM2-360M-Instruct` (~720 MB)

Total: ~3 GB. Worth mentioning to learners with disk-constrained machines.

### 6. Runtime

Total CPU time to run all 6 notebooks end-to-end: **~20-25 minutes** (most of it is LLM generation in NB 03, 04, assignment, and extensions). On GPU: 2-3 minutes.

## How to run a full re-verification

```bash
cd "<repo>/learner-edition/L10-transformers-genai/notebooks"
for nb in 01_monday_morning 02_attention_intuition 03_using_an_llm \
         04_rag_pipeline assignment optional_extensions; do
  OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \
    jupyter nbconvert --to notebook --execute "$nb.ipynb" \
    --output "$nb.ipynb" --ExecutePreprocessor.timeout=1500
done
```

## Module 3 progress — **COMPLETE**

| Lesson | Title | Option A status |
|--------|-------|-----------------|
| L01 | ML Intro & Foundations | ✅ Complete |
| L02 | Supervised Learning Basics | ✅ Complete |
| L03 | Making Models Work | ✅ Complete |
| L04 | Trees, Ensembles & Tuning | ✅ Complete |
| L05 | Unsupervised Learning | ✅ Complete |
| L06 | Time Series Forecasting | ✅ Complete |
| L07 | Neural Networks & DL Foundations | ✅ Complete |
| L08 | Computer Vision & CNNs | ✅ Complete |
| L09 | NLP & Embeddings | ✅ Complete |
| **L10** | **Transformers & GenAI** | **✅ Complete** |

**Module 3: 10/10 lessons complete.**

---

**Ready to teach.** L10's strength is connecting classical ML to modern GenAI through the SAME narrative thread (Sarah's journey). The closing slides (25-30) deliberately wrap up the entire module, treating L10 as both a lesson AND the course finale. The honest framing on LLM limits (hallucination, off-domain failure, prompt-vs-RAG-vs-fine-tune trade-offs) keeps learners grounded in production reality, not GenAI hype.
