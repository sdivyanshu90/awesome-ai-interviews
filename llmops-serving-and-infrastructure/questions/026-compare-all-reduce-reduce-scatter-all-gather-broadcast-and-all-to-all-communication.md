### Q: Compare all-reduce, reduce-scatter, all-gather, broadcast, and all-to-all communication.
* **Difficulty:** Senior
* **Category:** Distributed Systems
* **The 10-Second Pitch:** All-reduce aggregates and replicates; reduce-scatter aggregates then shards; all-gather concatenates shards to all ranks; broadcast copies one source; all-to-all exchanges distinct pieces among every rank.
* **The Deep Dive:** Ring all-reduce can be viewed as reduce-scatter plus all-gather with bandwidth-efficient chunks. DDP all-reduces gradients; FSDP reduce-scatters gradients and all-gathers parameters around compute. Tensor parallel uses all-reduce/all-gather/reduce-scatter depending row/column layout. MoE dispatch/return uses variable all-to-all. Latency depends on messages; bandwidth on total bytes; topology/contension determine tails.
* **Production Reality & Tradeoffs:** Overlap collectives with compute and bucket tensors, but too many small messages hurt. A stalled rank stalls synchronous collectives. Measure NCCL traces.
All-reduce computes a reduction and returns it to every rank, commonly implemented as reduce-scatter plus all-gather. Reduce-scatter leaves each rank one reduced shard; all-gather assembles shards; broadcast sends one source buffer; all-to-all sends distinct partitions between every pair and dominates expert routing. Model communication by bytes, latency steps, topology, and overlap—not collective name alone. Small messages are latency-bound; large messages approach link bandwidth and expose congestion/stragglers.

* **Red Flag:** Calling collectives equivalent because all move data.
