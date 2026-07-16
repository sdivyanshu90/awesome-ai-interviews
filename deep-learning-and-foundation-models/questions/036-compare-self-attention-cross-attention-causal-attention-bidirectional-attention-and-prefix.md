### Q: Compare self-attention, cross-attention, causal attention, bidirectional attention, and prefix attention.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Self versus cross identifies where Q and K/V originate; causal, bidirectional, and prefix identify visibility. These are orthogonal dimensions implemented through projections, memory, and masks.
* **The Deep Dive:** Self-attention derives Q/K/V from the same sequence/state. Cross-attention derives Q from one stream (decoder/query) and K/V from another memory (encoder/image/retrieved latents), so $T_q$ and $T_k$ differ and source K/V can be reused. Bidirectional self-attention allows all valid same-document pairs, suited to encoding. Causal self-attention allows key position $j\le i$, preserving autoregressive likelihood. Prefix attention allows a designated prefix to attend bidirectionally within itself while suffix tokens attend the full prefix and earlier suffix.

```text
bidirectional self:  [a b c] every token <-> every token
causal self:          a -> b -> c
cross:                decoder queries ---> fixed encoder K/V memory
prefix:              [source <-> source] => target1 -> target2
```

A decoder block may contain causal self-attention followed by cross-attention; “decoder” does not imply K/V always come from the same stream. Segment/document masks can refine any mode. During cache decoding, cross-memory cache is static while self KV grows.
* **Production Reality & Tradeoffs:** Cross-attention keeps source separate/citable but adds projections and memory. Concatenating modalities into self-attention is simpler but creates quadratic interactions and boundary/mask risks. State exact masks rather than relying on names.
* **Red Flag:** Equating self-attention with bidirectional attention or cross-attention with encoder-decoder architecture only.

