### Q: Compare causal LM, masked LM, prefix LM, span corruption, denoising, and encoder-decoder objectives.
* **Difficulty:** Senior
* **Category:** Foundation Models
* **The 10-Second Pitch:** Objectives determine visibility and prediction targets: causal LM predicts every next token; masked LM reconstructs hidden tokens bidirectionally; prefix LM combines bidirectional source and causal suffix; span/denoising reconstruct corrupted content, often with encoder-decoder separation.
* **The Deep Dive:** Causal LM maximizes $\sum_t\log p(x_t\mid x_{<t})$, matching open-ended generation and scoring every token after shift. Masked LM replaces a subset and predicts only masked positions using both sides; it learns representations but train-time masks do not appear naturally at inference. Prefix LM lets source/prefix tokens attend mutually while target tokens see prefix and earlier target, useful for conditional generation in one stack. Span corruption removes contiguous spans and predicts sentinel-delimited missing spans, emphasizing longer structure. General denoising corrupts through deletion, permutation, infilling, or noise and reconstructs clean data. Encoder-decoder objectives encode corrupted/source input bidirectionally and autoregressively decode target via cross-attention.

```text
causal:     x1 -> x2 -> x3 -> target next
masked:     x1 <-> [MASK] <-> x3 -> target hidden
prefix:     source <-> source => target -> target
span2span:  corrupted encoder input => removed spans decoder output
```
* **Production Reality & Tradeoffs:** Target density changes sample efficiency; corruption rate/type defines learned invariance. Encoder-decoder costs two stacks but may condition efficiently. Objective quality depends on downstream interface and data, not a universal winner. Keep evaluation serialization aligned.
* **Red Flag:** Saying masked LM is autoregressive because it predicts tokens, or ignoring that visibility masks—not model names—define the objective.

