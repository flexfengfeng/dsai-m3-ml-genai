# L09 — NLP, Embeddings & Semantic Search · Option A Handoff

**Status:** ✅ Complete
**Build date:** 2026-05-14
**Pattern:** Option A — slide-driven lecture (~90 min) + 4 in-class notebooks (~90 min), 🟡 Extension boundary separating Core from self-study.

---

## What's inside

```
L09-nlp-embeddings/
├── README.md                                Sarah's brief + Phase 1/2/3 structure
├── setup.md                                 Environment setup (sentence-transformers)
├── pre-class.md                             75-min self-study (Jay Alammar + NB01)
├── lesson.md                                Concept reference + Sarah's recommendation
├── reference.md                             22-term NLP/embedding glossary
├── environment.yml                          Conda env spec
├── slides/
│   ├── L09_slides.pptx                      28 slides, Ocean Gradient palette
│   └── slides_outline.md                    Slide-by-slide breakdown
├── notebooks/
│   ├── 01_monday_morning.ipynb              Pre-class — keyword search failure baseline
│   ├── 02_words_to_vectors.ipynb            One-hot → BoW → toy co-occurrence embedding
│   ├── 03_pretrained_embeddings.ipynb       all-MiniLM-L6-v2 + cosine + 2D PCA
│   ├── 04_semantic_search.ipynb             SemanticSearch class + TF-IDF benchmark + hybrid
│   ├── assignment.ipynb                     Recipe semantic-search engine
│   ├── optional_extensions.ipynb            Embeddings as features, multilingual, tokenisation
│   └── data/
│       ├── northstar_catalogue.csv          76 products, 10 categories (hand-crafted)
│       └── recipes.csv                      30 recipes, 6 categories (assignment dataset)
└── HANDOFF_OPTION_A.md                      (this file)
```

## Smoke-test results

All 6 notebooks executed end-to-end with `OMP_NUM_THREADS=1 MKL_NUM_THREADS=1`:

| Notebook | Cells | Status | Key result |
|----------|------:|--------|-----------|
| 01_monday_morning              | 22 | ✅ | Keyword baseline: 5/8 top-1 on 8-query benchmark |
| 02_words_to_vectors            | 30 | ✅ | One-hot all-zeros, toy embedding shows partial structure (cotton↔linen 0.378, frock↔dress 0.035) |
| 03_pretrained_embeddings       | 29 | ✅ | MiniLM lifts frock↔dress to 0.249; top-1 6/8 vs keyword 5/8 |
| 04_semantic_search             | 33 | ✅ | Semantic 6/8 vs TF-IDF 4/8 top-1; hybrid α=0.5 brings Lila Sundress into top-5 |
| assignment                     | 16 | ✅ | Recipe benchmark: Semantic 100%, TF-IDF 67% (clean gap) |
| optional_extensions            | 15 | ✅ | Classification 47% (small-data caveat), multilingual demo cross-language, tokenisation inspection |

## Narrative beats

1. **L08 → L09 bridge:** Marcus's customer-experience complaint — *"shopper typed 'blue summer dress', zero useful results"* — drives the entire lesson.
2. **The synonym failure:** Catalogue uses *frock / sundress / gown*. Keyword search finds nothing. NB01 makes this visceral.
3. **Embeddings introduction:** From one-hot (equidistant) to bag-of-words (still equidistant for synonyms) to co-occurrence (real but tiny) to pretrained (real and useful).
4. **The honest pretrained result:** MiniLM lifts synonym cosines from ~0 to 0.25-0.50 — meaningful but **not** the 0.7+ that learners might expect. Read cosines as a *ranking*, not a *percent similar*.
5. **Production realism:** Pure semantic gets 6/8 top-1 (vs keyword 5/8). The lesson does NOT oversell — production needs hybrid retrieval (TF-IDF + semantic + structured filters + re-rank).
6. **L09 → L10 bridge:** Where do embeddings come from? The answer is *attention + transformers* — L10 opens the black box.

## Deviations & notes for the instructor

### 1. Honest cosine values (critical pedagogical point)

The notebooks consistently emphasise: `all-MiniLM-L6-v2`'s cosine similarities live mostly in **[0.2, 0.7]**. Synonym pairs that are very close in meaning get ~0.25-0.50, not 0.9. Learners who interpret 0.25 as "25% similar" will be confused.

**Instructor framing:** read cosines as a *ranking signal*. A 0.25 vs 0.02 for unrelated is a real, useful difference. The absolute scale is a quirk of how the model was trained.

### 2. Top-1 isn't always perfect — and that's the lesson

On the 8-query benchmark, semantic gets 6/8 top-1, top-5 7/8. The two failures (*blue summer dress*, *lightweight rain jacket*) are NOT bugs — they're real cases where:
- The catalogue is small (76 products), so neighbouring categories are close in embedding space
- The query under-specifies (no colour filter applied) so the model can't break ties

Resolution: structured filters (category, colour) on top of semantic ranking. NB04 demonstrates the fix explicitly.

### 3. Assignment is cleanly graded

The recipe-corpus assignment shows a much cleaner result: semantic 100% top-1 vs TF-IDF 67%. The contrast between this and NB04's tighter 6/8 vs 4/8 is intentional — the assignment corpus is more semantically clustered, making the embeddings' advantage obvious.

### 4. Small-corpus classification result (Extensions NB)

The "embeddings as classifier features" experiment achieves only **47% accuracy** on a 10-class task with ~7 training examples per class per fold. The narrative explicitly explains why and notes that real-world accuracy on this kind of task with 50-200 examples per class would be 85-95%. Don't soften this — it teaches the right scaling intuition.

### 5. Model downloads

First-run downloads (one-time):
- `all-MiniLM-L6-v2`: ~80 MB
- `paraphrase-multilingual-MiniLM-L12-v2` (used only in optional extensions): ~470 MB
- Total cache after running all notebooks: ~600 MB in `~/.cache/huggingface/hub/`

Pre-warming for offline classrooms:
```bash
python -c "from sentence_transformers import SentenceTransformer; \
    SentenceTransformer('all-MiniLM-L6-v2'); \
    SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')"
```

### 6. Runtime budget

All 6 notebooks complete in **~3 minutes total** on CPU (the small models are cheap; the corpus is tiny). The assignment alone is ~1 minute. No special hardware required.

## How to run a full re-verification

```bash
cd "<repo>/learner-edition/L09-nlp-embeddings/notebooks"
for nb in 01_monday_morning 02_words_to_vectors 03_pretrained_embeddings \
         04_semantic_search assignment optional_extensions; do
  OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \
    jupyter nbconvert --to notebook --execute "$nb.ipynb" \
    --output "$nb.ipynb" --ExecutePreprocessor.timeout=600
done
```

Expected: all six complete in < 5 minutes total on a 2020-era MacBook Pro CPU.

## Module 3 progress

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
| **L09** | **NLP & Embeddings** | **✅ Complete (this lesson)** |
| L10 | Transformers & GenAI | ⏳ Next |

---

**Ready to teach.** Slides + notebooks + assignment + extensions all execute cleanly. The honest framing on cosine values and the hybrid-retrieval story keep this lesson grounded in production reality. Sarah's bridge to L10 — "where do embeddings come from?" — sets up the final lesson cleanly.
