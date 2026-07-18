### Q: Explain why quantization or speculation can reduce throughput on the wrong hardware/workload.
* **Difficulty:** Principal
* **Category:** Performance
* **The 10-Second Pitch:** Low-bit formats or draft work help only when they reduce the active bottleneck. Unsupported kernels, dequant overhead, small batches, compute-bound prefills, low acceptance, extra memory, or scheduling interference can make them slower.
* **The Deep Dive:** Weight-only quantization targets bandwidth-bound decode; large-batch prefill may be tensor-core compute-bound and use faster BF16/FP8 kernels. Irregular group sizes trigger fallback. Quantized KV adds scale/dequant and may reduce attention kernel efficiency. Speculation adds draft compute and target verification; low acceptance wastes work. Under high concurrency, ordinary batching may use target GPU better. Measure TTFT, ITL, goodput, power, and quality by workload.
* **Production Reality & Tradeoffs:** Hardware generations support formats differently. Memory savings may enable higher concurrency even if single-request latency worsens. Benchmark deployed engine versions.
Quantization can introduce dequantization/layout kernels, lose optimized tensor-core paths, or become compute-bound after bytes shrink. Speculation adds draft and verification work, variable accepted lengths, branch KV, and scheduler complexity. Both may improve single-request latency yet reduce aggregate goodput under saturation. Benchmark four quadrants—low/high concurrency and short/long outputs—with production lengths, quality gates, power, and cost. Always compare against a well-batched optimized baseline.

* **Red Flag:** Assuming an optimization's paper speedup transfers to every serving stack.
