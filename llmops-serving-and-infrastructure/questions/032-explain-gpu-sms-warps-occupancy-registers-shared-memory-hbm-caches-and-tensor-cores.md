### Q: Explain GPU SMs, warps, occupancy, registers, shared memory, HBM, caches, and tensor cores.
* **Difficulty:** Senior
* **Category:** GPU
* **The 10-Second Pitch:** SMs schedule warps of threads; registers/shared memory are fast on-chip resources; HBM is large/high-bandwidth off-chip memory; caches reduce traffic; tensor cores accelerate supported matrix formats. Performance is resource and memory-access dependent.
* **The Deep Dive:** Kernels launch thread blocks onto SMs. Warp divergence serializes paths. Register/shared-memory use limits resident blocks/warps and occupancy, but maximum occupancy is not always fastest. Coalesced global access and tiling move reusable data through L2/shared/registers. Tensor cores require aligned shapes/layout/dtypes. Latency hiding needs enough independent warps and instruction-level parallelism.
* **Production Reality & Tradeoffs:** Spills to local memory and bank conflicts hurt. GPU generations differ. Use counters and roofline, not rules of thumb alone.
An SM schedules warps; occupancy is active warps relative to hardware limits imposed by registers, shared memory, and blocks. High occupancy only helps hide latency and is not itself performance. Tensor cores accelerate supported matrix instructions; HBM holds bulk tensors; L2/shared memory/registers provide progressively smaller/faster reuse. Diagnose with achieved FLOPs, HBM bandwidth, cache hit rate, eligible warps, stalls, register spills, and launch gaps. Optimization means removing the active bottleneck, not maximizing every counter.

* **Red Flag:** Equating GPU utilization percentage with efficient SM/tensor-core use.
