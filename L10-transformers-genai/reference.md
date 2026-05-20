# L10 Reference · Glossary

| Term | Definition |
|------|------------|
| **Transformer**         | A neural-network architecture (Vaswani et al., 2017) built on stacked self-attention + feed-forward blocks. Foundation of every modern NLP and GenAI model. |
| **Attention**           | A mechanism for each token to look at every other token in the sequence, computing relevance-weighted information flow. |
| **Self-attention**      | Attention where queries, keys, and values all come from the same sequence (i.e., the token attends to itself and its neighbours). |
| **Query / Key / Value (Q/K/V)** | The three projections each token produces. Q seeks; K offers; V is the content. Attention = softmax(QKᵀ/√d) · V. |
| **Multi-head attention**| Running several self-attention computations in parallel, each capturing a different relationship pattern, then concatenating. |
| **Transformer block**   | One layer of the architecture: multi-head attention + residual + LayerNorm + feed-forward + residual + LayerNorm. |
| **Residual connection** | Adding a layer's input to its output (x + f(x)). Prevents gradient vanishing in deep networks. |
| **Layer normalisation** | A normalisation step applied per token across feature dimensions. Stabilises training. |
| **Positional encoding** | A way to inject token position into the input (since attention is order-invariant by default). |
| **Encoder**             | A transformer stack that produces a representation for each input token (BERT, embedding models). |
| **Decoder**             | A transformer stack that produces tokens one at a time, autoregressively (GPT family). |
| **Encoder-decoder**     | Both — e.g., T5, BART. Used for translation, summarisation. |
| **Autoregressive**      | Generating one token at a time, where each prediction depends on the previously generated tokens. |
| **LLM (Large Language Model)** | A large transformer trained on text. Modern usage = autoregressive (e.g., GPT, Llama, Claude). |
| **Instruction tuning**  | Training a base LLM on (instruction, response) pairs so it follows directions instead of just continuing text. |
| **Chat template**       | The specific token format an instruction-tuned model expects: special tokens marking user/assistant turns. |
| **Tokenisation**        | Splitting text into model-vocabulary tokens (same idea as L09). LLMs use BPE / SentencePiece. |
| **Temperature**         | Sampling parameter that scales the logits. T=0 → greedy (deterministic); T>1 → more random. |
| **Top-k sampling**      | Restrict sampling to the K highest-probability tokens. |
| **Top-p (nucleus) sampling** | Restrict sampling to the smallest set of tokens whose cumulative probability ≥ p. |
| **Hallucination**       | An LLM producing plausible-sounding but factually wrong output. Not a bug — a consequence of training. |
| **RAG (Retrieval-Augmented Generation)** | Pipeline that retrieves relevant docs by embedding similarity and stuffs them into the LLM's context. Fixes hallucination on YOUR data. |
| **System prompt**       | The first message in a chat conversation, used to set behaviour (persona, rules, allowed actions). |
| **Fine-tuning**         | Further training of a pretrained model on a domain-specific dataset. Last resort for GenAI — try prompting and RAG first. |
| **HuggingFace pipeline** | The simplest API to use a pretrained model: `pipeline("task-name")` returns a callable. |

---

## Further reading & watching

Optional resources that go deeper than what we cover in class. Pick what interests you; nothing here is required.

### Videos

- [Attention in transformers, step by step](https://www.youtube.com/watch?v=eMlx5fFNoYc) — 3Blue1Brown, ~25 min — *The mechanism behind every modern LLM. Visual, slow, worth the time.*

### Reading

- **Andrej Karpathy — *State of GPT*** — YouTube or talk slides — *Pick any 10-minute section. The history of how GPT-1 became GPT-4 in 6 years.*
