### Q: Compare RoPE interpolation/scaling, continued long-context training, memory tokens, and retrieval.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** RoPE methods alter positional frequencies, continued training teaches behavior at length, memory tokens compress selected state inside the model, and retrieval moves long-term information outside context. They solve different positional, learning, state, and freshness problems.
* **The Deep Dive:** Position interpolation maps longer indices into the pretrained range, preserving familiar phases but compressing local distances. Base/NTK-aware/dynamic/piecewise RoPE scaling changes frequencies to preserve more local behavior while extending slower dimensions; no rescaling teaches the model to ignore new distractors. Continued long-context training exposes scaled positions and long dependencies, requiring length curriculum/data, large memory, and position-balanced tasks. Memory tokens or recurrent summaries condense earlier segments into a bounded learned state; they reduce cache but may discard exact facts and need training for write/read. Retrieval indexes external chunks and inserts selected evidence, enabling far larger/fresh corpora and citations but adding retrieval misses, injection, latency, and ACL/freshness complexity.

```text
RoPE scaling: change coordinate system
continued training: learn on new lengths
memory tokens: compress prior internal context
retrieval: select external evidence into context
```
* **Production Reality & Tradeoffs:** Often combine methods: moderate native window + scaling/training + retrieval. Evaluate exact/local recall, natural multi-hop, distractors, evidence positions, citation, TTFT/KV, and updates/deletion. Configured length is not effective length.
* **Red Flag:** Treating retrieval as equivalent to more context, or applying RoPE scaling and claiming the model is long-context trained.

