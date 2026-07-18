### Q: Explain DDP, FSDP, ZeRO stages, sharded optimizers, and activation checkpointing.
* **Difficulty:** Senior
* **Category:** Distributed Training
* **The 10-Second Pitch:** DDP replicates model/optimizer and synchronizes gradients. ZeRO/FSDP progressively shard optimizer state, gradients, and parameters. Activation checkpointing discards selected forward activations and recomputes them in backward.
* **The Deep Dive:** ZeRO-1 shards optimizer, stage 2 gradients too, stage 3 parameters too. FSDP wraps modules, all-gathers parameter shards before compute, reduce-scatters gradients after, and reshares. Prefetch/overlap and wrap granularity trade communication/memory. DDP buckets all-reduce gradients. Checkpointing saves boundary inputs/RNG and reruns interiors, increasing FLOPs while reducing activation memory.
* **Production Reality & Tradeoffs:** Sharding small modules causes tiny collectives; large modules increase peak gather memory. Checkpointing interacts with dropout/RNG and compiler. State-dict save/load needs distributed formats.
DDP replicates parameters/optimizer and all-reduces gradients. ZeRO-1 shards optimizer state, ZeRO-2 also shards gradients, and ZeRO-3/FSDP shards parameters with all-gather/reduce-scatter around use. Activation checkpointing discards selected activations and recomputes them backward. Memory savings trade communication, recompute, and more failure-sensitive orchestration. Parameter prefetch order, wrapping policy, mixed precision, and checkpoint format determine whether theoretical savings materialize.

* **Red Flag:** Saying FSDP reduces total cluster memory or compute rather than per-rank redundancy.
