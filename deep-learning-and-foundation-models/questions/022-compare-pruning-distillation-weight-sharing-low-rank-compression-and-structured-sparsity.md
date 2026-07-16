### Q: Compare pruning, distillation, weight sharing, low-rank compression, and structured sparsity.
* **Difficulty:** Senior
* **Category:** Deployment
* **The 10-Second Pitch:** Compression methods remove weights, transfer behavior, reuse parameters, factor matrices, or impose hardware-supported sparsity. Parameter/FLOP reduction only becomes latency/cost reduction when kernels, memory layout, and workload exploit the structure.
* **The Deep Dive:** Unstructured pruning zeros individual weights and may compress storage, but irregular sparse kernels need high sparsity to beat dense hardware. Structured pruning removes channels, heads, neurons, layers, or blocks, yielding smaller dense shapes but potentially larger quality loss. N:M sparsity enforces fixed local patterns supported by some accelerators. Distillation trains a student on teacher logits/representations/sequences, with temperature controlling soft-target entropy; it can change architecture but inherits teacher errors and needs representative transfer data. Weight sharing ties or quantizes weights to codebook values, reducing unique storage. Low-rank compression replaces $W\approx UV$ with cost $r(d_{in}+d_{out})$ and two matmuls, profitable only when rank is sufficiently small and kernels efficient.

| Method | Reduces | Needs retraining? | Hardware dependence |
|---|---|---:|---:|
| pruning | weights/shape | usually | high |
| distillation | whole model | yes | low |
| low rank | matrix rank | usually | medium |
| sharing | storage | often | medium |
* **Production Reality & Tradeoffs:** Combine only after ablations because errors compound. Benchmark end-to-end TTFT/ITL/throughput/memory at matched quality, including dequant/decompression and batching. Rare/safety capabilities often regress before average benchmarks.
* **Red Flag:** Reporting theoretical sparsity or parameter count as production speedup without supported kernels and measured workload.

