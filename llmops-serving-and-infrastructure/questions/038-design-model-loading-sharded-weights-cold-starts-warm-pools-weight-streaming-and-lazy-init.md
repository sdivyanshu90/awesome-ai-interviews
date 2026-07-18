### Q: Design model loading, sharded weights, cold starts, warm pools, weight streaming, and lazy initialization.
* **Difficulty:** Principal
* **Category:** Serving
* **The 10-Second Pitch:** Store immutable sharded weights aligned to parallel layout, load/verify in parallel, preallocate runtime/KV resources, and maintain warm capacity. Streaming/lazy methods trade readiness latency against memory/complexity.
* **The Deep Dive:** Registry manifest maps tensors/shards/digests/model/tokenizer/config. Workers download/cache locally, verify, deserialize safely, and place directly to target ranks/dtypes to avoid host copies. Warm pools keep engines compiled and weights resident. Weight streaming overlaps IO with initialization or serves layers from tiers only for specialized cases. Health checks run known prompts before admission.
* **Production Reality & Tradeoffs:** Cold start includes image pull, weights, compilation, graph capture, NCCL, and warmup. Warm pools cost idle GPUs. Coordinate rollouts to avoid storage/network storms.
Loading involves artifact verification, shard download/read, deserialization, allocation, optional dequantization/engine build, rank synchronization, and warmup. Store topology-aligned shards in local or nearby caches, use checksummed parallel reads, and maintain warm pools sized by demand/cold-start SLO. Weight streaming trades storage for startup and can bottleneck network. Lazy initialization defers cost and may move the spike to the first user; readiness should require kernels/graphs/KV pools actually warmed.

* **Red Flag:** Calling container startup the whole model cold start.
