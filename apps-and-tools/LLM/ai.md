# 💅 LLM

#### Therms

* GPT - generative pre-trained transformer
* SSL - self supervised learning (training process)
* AI - Artificial Intelligence
* ML - Machine Learning
* NLP - Natural Language Processing
* LLM - Large Language Models

## Tokenization

LLMs read  token IDs. Text is split via BPE (Byte-Pair Encoding): frequent character pairs get merged into single tokens until vocabulary size is reached (~50k–100k tokens).

- Average: ~4 characters per token, ~0.75 words per token in English
- Emoji, symbols, non-English text = more tokens per unit of meaning
- Token boundaries cause blind spots (e.g. letter-counting failures)
- Cost and context limits are both measured in tokens

## Transformers & Attention

### The core problem 

After tokenization you have a sequence of integers. But meaning depends on relationships between tokens — "bank" means something different next to "river" vs "loan". Transformers solve this.

### Self-attention

For every token the model learns three vectors: 
- Query ("what am I looking for?")
- Key ("what do I offer?")
- Value ("what do I contribute if selected?"). 
Each token's Query is scored against every other token's Key — high match = high attention weight. The token's output is then a blend of all Values weighted by those scores. This is how "it" gets resolved to "animal" — "animal" scores highest against "it"'s Query given the surrounding context.

### Why this matters

Attention is computed in parallel across all tokens simultaneously — that's what made Transformers faster than previous architectures which processed tokens one at a time.
Multi-head attention: the model runs many attention passes in parallel (e.g. 96 heads in large models), each learning different relationship types — syntax, coreference, position. Outputs are concatenated and merged. More heads = richer understanding of relationships.

### O(n²) cost

Every token attends to every other token. Double the context = 4x the compute. This is why long contexts are expensive and why context is a resource to manage actively.

### Context window

Hard architectural limit — the maximum tokens the model can hold in attention at once. Everything outside it is gone completely — no skimming, no background memory. 

Claude models: 200k tokens (~150k English words). Fills fast in agentic workflows with long histories and tool outputs.


### Temperature

After all attention layers run, the model outputs a probability distribution over the full vocabulary for the next token. Temperature controls sampling from that distribution 
- 0 always picks the top token (deterministic, loop risk)
- 1 samples proportionally (balanced)
- 1 flattens the distribution (creative but unstable).

### Two-part mental model

LLM system = context (your text, history, docs) + compute (model inference). The model is fully stateless — you pass the full context every call, it processes and returns, remembers nothing.

- Local inference: model weights + KV cache in VRAM — 24GB+ for large contexts
- Cloud API: you send ~800KB of plain text for 200k tokens — weights and compute stay on provider infra, only text travels the wire
