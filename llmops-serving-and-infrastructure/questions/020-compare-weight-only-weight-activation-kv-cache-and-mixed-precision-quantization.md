### Q: Compare weight-only, weight-activation, KV-cache, and mixed-precision quantization.
* **Difficulty:** Senior
* **Category:** Quantization
* **The 10-Second Pitch:** Weight-only reduces model bandwidth/storage; weight-activation enables low-bit tensor-core compute; KV quantization expands context/concurrency; mixed precision assigns formats per component based on sensitivity.
* **The Deep Dive:** Weight-only INT4 dequantizes during GEMM and helps decode. W8A8 quantizes activations with per-token/channel scales and outlier handling. KV cache may use FP8/INT8/INT4 with head/block scales; errors affect attention over long context. Embeddings, norms, logits, router, and selected layers often stay higher precision. Calibration maps ranges/clipping.
* **Production Reality & Tradeoffs:** Speed needs supported kernels and enough batch/shape. Metadata/dequant overhead can erase gains. Evaluate perplexity plus long-context, code, multilingual, tool, and safety slices.
Weight-only quantization compresses weights but computes activations at higher precision and mainly helps weight-bandwidth-bound decode. Weight-activation quantization also reduces activation traffic/tensor-core operands but needs calibration and outlier handling. KV quantization targets context-dependent memory/bandwidth and can accumulate long-context quality loss. Mixed precision keeps sensitive layers/channels higher precision. Report effective bytes including scales/zeros, dequantization work, kernel support, and accuracy by length/tool/rare-token slice.

* **Red Flag:** Equating bit-width reduction with proportional latency reduction.
