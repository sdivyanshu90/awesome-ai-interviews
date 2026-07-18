### Q: Diagnose when compilation, custom kernels, or graph capture regress rather than improve performance.
* **Difficulty:** Principal
* **Category:** Performance
* **The 10-Second Pitch:** Compare stage traces and hardware counters against a correct eager baseline. Regressions arise from compile overhead, graph breaks, bad shapes/layout, padding, fusion pressure, fallback, synchronization, or small workloads.
* **The Deep Dive:** Separate cold build/startup from steady state. Inspect generated graph/kernel selection, recompilations, breaks, CPU launch gaps, memory traffic, occupancy, registers/spills, tensor-core use, and syncs. Sweep batch/length. Validate precision/correctness because a faster wrong kernel is irrelevant. Check concurrent workload interference and allocator changes.
* **Production Reality & Tradeoffs:** Optimizations can move bottlenecks to tokenization/network/scheduler. Keep automatic fallback and versioned benchmarks. Do not tune only average throughput.
Regressions usually come from graph breaks/recompilation, shape bucketing/padding, kernel autotuning mismatch, register pressure, layout conversions, synchronization, or a workload too small to amortize compile/capture. Compare a stage trace against eager, then inspect generated kernel count, HBM traffic, occupancy, spills, and host gaps. Warm both paths and include cold-start/build time separately. Disable one optimization at a time; stacking compilation, custom kernels, and graphs makes attribution otherwise impossible.

* **Red Flag:** Assuming lower kernel microbenchmark time guarantees lower p99.
