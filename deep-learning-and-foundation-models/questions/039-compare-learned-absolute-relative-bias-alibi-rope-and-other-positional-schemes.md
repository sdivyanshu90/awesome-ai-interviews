### Q: Compare learned absolute, relative bias, ALiBi, RoPE, and other positional schemes.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Absolute schemes add position to token states; relative bias alters attention logits by distance; ALiBi adds head-specific linear distance penalties; RoPE rotates Q/K so dot products depend on relative offsets. Their extrapolation and local-resolution behavior differ.
* **The Deep Dive:** Learned absolute embeddings add $e_{pos}$ and are flexible within a maximum table but need resize/interpolation outside it. Sinusoidal absolute vectors are fixed and defined beyond training. Relative-position bias adds $b_h(i-j)$—learned per distance/bucket—to scores, directly representing distance and often clipping/bucketing far ranges. ALiBi uses $-m_h(i-j)$ for causal distance with different slopes per head, requiring no position state and favoring recency. RoPE rotates feature pairs by position and produces relative phase in QK while retaining absolute effects through content.

Other schemes include convolutional position, relative key/value embeddings, T5 buckets, and learned interpolation/scaling. Compare whether position enters residual, Q/K, or logits; parameter/cache implications; bidirectional/causal support; and behavior beyond trained length.

| Scheme | Injection point | Extrapolation issue |
|---|---|---|
| learned abs | residual | table/unseen IDs |
| relative bias | logits | bucket/clipping |
| ALiBi | logits | excessive far penalty |
| RoPE | Q/K | phase/frequency shift |
* **Production Reality & Tradeoffs:** No scheme alone solves lost-in-middle or long-context reasoning. Scaling RoPE trades local resolution; ALiBi may suppress distant retrieval; bias tables add memory. Evaluate lengths, positions, local tasks, and distractors.
* **Red Flag:** Ranking schemes by configured maximum context instead of measured behavior, or treating every relative method as RoPE.

