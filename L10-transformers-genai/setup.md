# L10 Setup

## Prerequisites

You should have the `dsai-m3` conda environment from L01-L09. If not, see L01's setup.md.

## New dependencies

Everything L10 needs was installed in L09 (`transformers`, `sentence-transformers`, `torch`). No new packages.

## Model downloads

L10 uses three pretrained models from Hugging Face. They download automatically on first use (one-time):

| Model | Size | Used in |
|-------|------|---------|
| `distilbert-base-uncased-finetuned-sst-2-english` | ~268 MB | NB 01 sentiment |
| `dslim/bert-base-NER` | ~436 MB | NB 01 named-entity recognition |
| `HuggingFaceTB/SmolLM2-360M-Instruct` | ~720 MB | NB 03, NB 04, assignment |
| `all-MiniLM-L6-v2` (already cached from L09) | ~80 MB | NB 04 retrieval step |

Total disk: ~1.5 GB in `~/.cache/huggingface/hub/`.

### Pre-warming for offline classrooms

```bash
python -c "
from transformers import pipeline, AutoTokenizer, AutoModelForCausalLM
from sentence_transformers import SentenceTransformer
pipeline('sentiment-analysis')
pipeline('ner', model='dslim/bert-base-NER')
AutoTokenizer.from_pretrained('HuggingFaceTB/SmolLM2-360M-Instruct')
AutoModelForCausalLM.from_pretrained('HuggingFaceTB/SmolLM2-360M-Instruct')
SentenceTransformer('all-MiniLM-L6-v2')
"
```

## macOS note (PyTorch threading)

Same as L07-L09. If a kernel crashes during generation:

```bash
OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 jupyter notebook
```

The notebooks call `torch.set_num_threads(1)` at the top.

## CPU is fine

All notebooks run on CPU. SmolLM2-360M generates ~5-15 tokens/second on a 2020-era MacBook — slow but usable. If you have a GPU, generation is 20-50× faster.

## Sanity check

```bash
python -c "
from transformers import pipeline
s = pipeline('sentiment-analysis')
print(s('This module was excellent'))
"
```

Expected output: `[{'label': 'POSITIVE', 'score': 0.99...}]`
