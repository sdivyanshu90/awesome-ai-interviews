### Q: Profile with framework traces, Nsight Systems, Nsight Compute, NCCL telemetry, and GPU counters.
* **Difficulty:** Principal
* **Category:** Performance
* **The 10-Second Pitch:** Start with end-to-end traces, narrow to kernels/collectives, then inspect hardware counters. Correlate all ranks and request stages so optimization targets user-visible bottlenecks.
* **The Deep Dive:** Framework profiler maps ops/shapes/allocations. Nsight Systems shows CPU/GPU timelines, launches, streams, NCCL, gaps. Nsight Compute inspects selected kernel occupancy, memory throughput, tensor cores, stalls, and instructions. NCCL logs/topology and fabric counters reveal collective paths. Add NVTX/request IDs. Profile representative warm workloads and sample enough tails without perturbing heavily.
* **Production Reality & Tradeoffs:** Detailed profiling adds overhead and huge traces. Reproduce before microanalysis. Protect prompt metadata. Compare against baselines after every change.
Use framework traces to locate operators and CPU gaps, Nsight Systems to correlate CPU/CUDA/NCCL timelines, Nsight Compute for kernel-level occupancy/memory/instruction metrics, and fabric telemetry for link congestion/errors. Profile a representative warmed window with NVTX/request/rank annotations; excessive profiler detail perturbs timing. Form a hypothesis from the coarse trace, collect only counters needed to test it, then reproduce improvement end to end. Never infer GPU saturation from utilization percentage alone.

* **Red Flag:** Opening Nsight Compute first without knowing which kernel matters.
