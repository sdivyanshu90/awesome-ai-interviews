### Q: Why have efficient-attention alternatives not universally replaced exact softmax attention?
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Exact attention offers flexible content-addressable retrieval, mature fused kernels, and predictable quality. Alternatives save asymptotic work only after crossover and often trade approximation, sparsity assumptions, state compression, training complexity, or hardware efficiency.
* **The Deep Dive:** Dense softmax can route each query to arbitrary tokens with a normalized, data-dependent distribution. FlashAttention removes quadratic intermediate IO while keeping exact semantics, raising the length at which algorithmic alternatives win. Local/block-sparse attention loses edges unless patterns and depth connect needed evidence. Kernel/linear attention approximates or changes softmax and can have unstable normalization or feature dimensions. Recurrent/SSM approaches compress history, improving streaming state but weakening arbitrary recall. Low-rank methods assume attention matrices are compressible for the task.

Asymptotic $O(T)$ or $O(T\log T)$ does not guarantee latency: small/medium sequences, constant factors, extra projections, poor occupancy, dynamic shapes, and immature kernels can make dense faster. Existing checkpoints, TP/serving stacks, quantization, KV/prefix caches, and tooling are optimized for attention. Quality regressions on rare long-copy/tool/code tasks can outweigh average perplexity gains. Natively-trained sparse attention (NSA/MoBA-class) learns hardware-aligned block selection during pretraining rather than retrofitting sparsity, and the standard production pattern interleaves sliding-window layers with periodic full-attention layers (Gemma-2/3, gpt-oss style), retaining exact global retrieval in a fraction of layers.

A fair comparison matches parameters, training FLOPs/tokens/data, context, hardware, and quality; plots latency/memory versus length and includes long-context stress suites.
* **Production Reality & Tradeoffs:** Alternatives may dominate specific streaming/edge/very-long workloads and hybrids can capture most benefit. Exact attention still has quadratic prefill and growing KV, so this is workload-dependent, not permanent.
* **Red Flag:** Arguing from big-O alone or using short-context perplexity as proof of replacement.

