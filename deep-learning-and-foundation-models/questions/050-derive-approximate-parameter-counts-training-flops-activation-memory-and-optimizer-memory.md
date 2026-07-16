### Q: Derive approximate parameter counts, training FLOPs, activation memory, and optimizer memory for an LLM.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** For a dense decoder, nonembedding parameters are roughly $L(4D^2+3DF)$ for QKV/O and gated MLP with width $F$; training FLOPs are about six times parameters times tokens when matmuls dominate, while attention, embeddings, activations, optimizer state, and parallel buffers require separate accounting.
* **The Deep Dive:** Per layer MHA projections Q/K/V/O contribute about $4D^2$ (GQA lowers K/V terms). SwiGLU with gate/up/down contributes $3DF$. Norm/bias terms are lower order. Embedding/output add $VD$ once if tied, twice if untied. Thus $P\approx L(4D^2+3DF)+VD$.

A matrix parameter participates in one multiply per forward token and roughly two in backward (activation and weight gradients), each multiply-add counted as two FLOPs: approximately $6P_{matmul}N_{tokens}$ for training. Dense attention score/value adds roughly $4LBT^2D$ forward and corresponding backward, material at long $T$. Inference prefill/decode must be modeled separately.

Weights use $Ps_w$ bytes. Adam mixed-precision training may store low-precision weights, FP32 master weights, FP32 gradients, and two FP32 moments—commonly around 16 bytes/parameter before sharding, but implementation varies. Activations scale with $LBTD$ times saved tensors; checkpointing reduces saved layers by recomputation.

```text
memory = weights + gradients + optimizer + activations + temporary/collective buffers
```
* **Production Reality & Tradeoffs:** “6P tokens” is an approximation, not capacity planning. MoE total versus active parameters, sequence length, vocab logits, recomputation, precision, ZeRO/FSDP/TP/PP, padding, and utilization change totals. Confirm with profiler and allocator peaks.
* **Red Flag:** Estimating training memory from weight dtype alone or ignoring $T^2$ attention at long context.

