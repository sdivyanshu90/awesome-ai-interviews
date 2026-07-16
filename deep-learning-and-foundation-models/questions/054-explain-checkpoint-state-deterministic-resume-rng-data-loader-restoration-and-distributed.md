### Q: Explain checkpoint state, deterministic resume, RNG/data-loader restoration, and distributed consistency.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** A resumable checkpoint is a transactional snapshot of model, optimizer, scaler, scheduler, RNG, data position, and distributed/sharding metadata. Commit a verified global manifest last and test that resumed loss/updates match an uninterrupted run.
* **The Deep Dive:** Save logical model parameters/buffers, optimizer moments and step, master weights, gradient scaler, LR schedule, global tokens/updates, clipping/EMA states, and algorithm-specific router/adapters. Capture Python/CPU, NumPy, each device/rank RNG plus dropout/data-augmentation generators. Data state includes dataset/mixture/tokenizer versions, shard/sample/token cursor, sampler permutation, packing buffers, and worker state. Distributed metadata records world size, mesh/process groups, tensor/expert/pipeline/FSDP layouts and parameter mapping.

Each rank writes temporary content-addressed shards and checksums; a coordinator writes global metadata only after all shards succeed, then atomically publishes the manifest. Load validates completeness/digests and either restores the same mesh or reshares logical tensors. Restore RNG before any stochastic operation. Compare the next several batches, losses, gradients, and updates with an uninterrupted control.

```text
rank temp shards -> verify all -> manifest commit -> durable checkpoint
partial shards without manifest are never resumable
```
* **Production Reality & Tradeoffs:** Bitwise determinism may be impossible with nondeterministic kernels/world-size changes, but statistical continuation is not permission to omit state. Checkpoint IO can stall training; async snapshots need consistency/copy-on-write. Regularly disaster-test corruption and resharding.
* **Red Flag:** Saving weights only or considering a directory valid because most rank files exist.

