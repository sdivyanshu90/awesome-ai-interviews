### Q: Why is prefill compute-bound while decode is commonly memory-bandwidth-bound?
* **Difficulty:** Senior
* **Category:** Systems
* **The 10-Second Pitch:** Prefill performs large matrix multiplications over many prompt tokens, achieving high arithmetic reuse; decode processes one token per sequence and repeatedly streams model weights/KV, yielding low arithmetic intensity.
* **The Deep Dive:** For a linear layer, prefill GEMM `[BT,D]×[D,F]` reuses each weight across many token rows. Decode GEMV/small GEMM has few rows, so bytes moved per FLOP are high. Attention prefill computes quadratic token interactions with efficient tiled kernels; decode computes one query over cached K/V and is dominated by cache reads at long context. Batching increases decode rows and weight reuse until KV/scheduler limits.
* **Production Reality & Tradeoffs:** Short prompts, tiny models, huge batches, MoE, and hardware alter the boundary. Roofline/profile rather than assume. Prefill/decode disaggregation can specialize hardware but transfers KV.
A roofline explanation is precise: prefill uses large matrix multiplications across sequence positions, giving high reuse of weights and high arithmetic intensity; decode at small batch repeatedly streams the same weights plus growing KV for few FLOPs per sequence. Decode can become compute-bound with sufficiently large batches, small/quantized weights, or expensive architectures, while long-context attention may be KV-bandwidth bound. Measure achieved FLOP/s and HBM bytes rather than treating the rule as universal.

* **Red Flag:** Calling decode compute-free or prefill always the latency bottleneck.
