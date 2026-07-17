### Q: How do linear projectors, resamplers, Q-Former, perceiver resampling, and gated cross-attention connect vision to an LLM?
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Connectors translate variable visual features into the LLM's width and token budget. Linear/MLP projectors preserve many tokens; resamplers compress them; Q-Former uses learned queries; gated cross-attention injects visual context into selected language layers.
* **The Deep Dive:** Let the vision encoder emit $V\in\mathbb R^{B\times N_v\times d_v}$ and the LLM expect width $d_l$. A projector computes $P=VW$ with $W\in\mathbb R^{d_v\times d_l}$, then inserts all $N_v$ vectors as prefix or interleaved language tokens. A resampler owns $M$ learned latent queries and performs latent-to-vision cross-attention, producing fixed $M\ll N_v$ outputs regardless of input resolution. Perceiver resampling repeats latent self- and cross-attention; a Q-Former often adds contrastive, image-text matching, and generation pretraining so queries select language-relevant evidence. Gated cross-attention keeps visual memory external: selected LLM layers add $\tanh(g)\operatorname{CrossAttn}(H,V)$ to the residual stream. Initializing $g\approx0$ preserves the pretrained language path at initialization.

```text
image -> vision encoder -> N_v visual features
                              |
             +----------------+----------------+
             |                                 |
       projector: N_v                    resampler: M << N_v
             |                                 |
       [visual | text] -> LLM       [compressed visual | text] -> LLM

alternative: text states -> gated cross-attention -> external visual memory
```

  For multiple images, preserve boundary, order, region, and timestamp provenance explicitly; position indices alone do not establish which image supports an answer. The connector must align scale and distribution with the LLM embedding space and is an information bottleneck even when tensor shapes match.
* **Production Reality & Tradeoffs:** Projection is simple and high-fidelity but visual tokens increase prefill and KV cost. Compression cuts cost yet can discard tiny text, objects, or exact spatial relationships; dynamic resampling can spend more tokens on dense pages. Rich connectors require more pretraining and complicate quantization, batching, and prefix caching. Compare quality at matched token, memory, and latency budgets using OCR, counting, and grounding—not captions alone.
* **Red Flag:** Saying the projector merely changes tensor shape with no representational bottleneck.
