### Q: Design fault-tolerant distributed checkpointing, restart, resharding, and elastic recovery.
* **Difficulty:** Principal
* **Category:** Reliability
* **The 10-Second Pitch:** Write sharded state transactionally with a manifest/checksums, retain optimizer/RNG/data position, and support topology-aware resharding. Recovery selects a complete checkpoint and prevents duplicated/skipped data/effects.
* **The Deep Dive:** Ranks write immutable shards to staging; manifest commits only after validation. Metadata describes global tensors, shard offsets, parallel topology, versions, step/tokens, RNG, sampler, scaler, and scheduler. Async save snapshots consistent state or uses copy-on-write. Reshard loader maps global tensor ranges to new ranks. Elastic recovery coordinates membership and restarts at a safe boundary.
* **Production Reality & Tradeoffs:** Checkpoint IO can stall training and overwhelm storage. Test corruption, partial uploads, node loss, and changed world size. Bitwise determinism may not survive topology change.
Checkpoint atomically records model/optimizer/RNG/scheduler/data-cursor state plus topology-independent metadata and per-shard checksums. Ranks write temporary shards, a coordinator verifies a manifest, then publishes one commit marker. Restart ignores incomplete generations. Resharding reconstructs logical tensors into a new world size without loading the full model on one host. Test corruption, missing ranks, preemption during commit, and deterministic data resumption; a checkpoint that loads but repeats/skips training data is not correct.

* **Red Flag:** Saving only model weights and calling training fault tolerant.
