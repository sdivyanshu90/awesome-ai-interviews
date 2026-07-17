### Q: How do filters, sharding, replication, updates, and tombstones affect ANN recall and consistency?
* **Difficulty:** Principal
* **Category:** Distributed Systems
* **The 10-Second Pitch:** ANN assumes a searchable candidate population; metadata filters, shards, replicas, and asynchronous mutations change that population. Correct systems define authorization/freshness semantics and compensate for recall loss through filter-aware indexing, oversampling, or routing.
* **The Deep Dive:** Postfiltering top-k can return too few authorized items; prefiltering may break graph traversal or cluster efficiency. Sharding requires query fan-out and global top-k merge; semantic or tenant routing risks missed shards. Replicas improve availability but can serve different index versions. Inserts may be visible before graph optimization; deletes use tombstones until compaction. Versioned snapshots and write-ahead metadata coordinate vector, lexical, and document stores.
* **Production Reality & Tradeoffs:** Fail closed for ACLs even if recall drops. Measure recall during churn, replica lag, and compaction. Strong consistency is expensive; state the freshness SLA.
Global top-$k$ over $S$ shards requires each shard to return enough candidates for a coordinator merge; routing to fewer shards lowers cost but can miss neighbors. Replica/index versions must be observable in results. Model insert/delete visibility as explicit states—document store committed, embeddings pending, index searchable, tombstoned, compacted—and define which the API may serve. Authorization is a hard invariant even during replica lag or partial failure.

* **Red Flag:** Applying a filter after top-k and assuming recall is unchanged.
