### Q: How do expert parallelism and all-to-all communication constrain MoE scaling?
* **Difficulty:** Principal
* **Category:** Distributed Systems
* **The 10-Second Pitch:** Expert parallelism shards experts across ranks, so every MoE layer packs and all-to-all dispatches token activations, runs uneven grouped GEMMs, then all-to-all returns them. Fabric bandwidth/latency and routing skew can dominate sparse compute savings.
* **The Deep Dive:** With tokens shaped $[N,D]$, top-k routing creates up to $Nk$ token-expert assignments. Each rank groups activations by destination expert/rank, exchanges variable-size payloads with all-to-all, executes local expert batches, then reverses the exchange and gate-aggregates. Communication volume per layer is proportional to assignments times hidden width/dtype in each direction, plus metadata; total expert weights remain sharded.

Place expert groups within fast NVLink/NVSwitch or low-oversubscription domains where possible. Tokens per local expert must be large enough for efficient GEMMs, creating tension between many experts and microbatch size. Routing skew causes different message sizes, stragglers, capacity drops/padding, and memory imbalance. Tensor/sequence/context parallel collectives may contend with all-to-all; schedule/overlap only helps if links and dependencies permit.

```text
rank tokens -> pack by expert -> all-to-all -> grouped expert GEMMs -> reverse all-to-all -> combine
                   skew/network          compute imbalance          network
```
* **Production Reality & Tradeoffs:** Measure all-to-all p50/p99, effective bandwidth, expert batch histograms, exposed overlap, and step stragglers on the real topology. Hierarchical routing/experts reduce cross-node traffic but constrain choice. Checkpoint/resume must reshard experts.
* **Red Flag:** Sizing MoE only from active FLOPs or assuming nominal interconnect bandwidth is available to every concurrent collective.

