### Q: Explain kernel fusion, FlashAttention, fused MLP/norm/sampling, and memory-layout effects.
* **Difficulty:** Senior
* **Category:** GPU
* **The 10-Second Pitch:** Fusion keeps intermediates on-chip and reduces launches/HBM traffic. FlashAttention tiles exact attention with online softmax; fused norm/MLP/sampling combine bandwidth-heavy operations. Layout determines coalescing and tensor-core eligibility.
* **The Deep Dive:** Unfused residual+norm writes/reads full tensors; fused kernels compute in registers/shared memory. FlashAttention avoids materializing `T×T` scores in HBM and recomputes in backward. Fused gated MLP joins projections/activation where possible; sampling can fuse penalties, softmax/top-k. Contiguous strides, alignment, head dimension, packing, and quantization groups decide kernel path.
* **Production Reality & Tradeoffs:** Fusion can increase register pressure, reduce occupancy, complicate numerics, and lose benefit for small shapes. Verify outputs/tolerances and profile, not assume.
Fusion reduces kernel launches and intermediate HBM round trips: norm, projection/bias/activation, attention, or sampling stages can remain in registers/shared memory. FlashAttention is an exact tiled attention algorithm that avoids materializing the full score matrix; it is not sparse attention. Gains depend on shape, dtype, layout, and occupancy. A fused kernel can regress through register spilling, poor small-shape utilization, extra padding, or unsupported masks, so inspect kernel time and memory traffic.

* **Red Flag:** Calling fewer kernels automatically faster.
