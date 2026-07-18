### Q: Design an embedding platform with online/offline consistency, versions, backfills, and migration.
* **Difficulty:** Principal
* **Category:** Embedding Platform
* **The 10-Second Pitch:** Expose a versioned embedding contract covering model, preprocessing, task prefix, dimension, normalization, and metric; run the same artifact online/offline and migrate with shadow backfill plus dual read/write.
* **The Deep Dive:** Registry publishes immutable embedding bundle and compatibility metadata. Batch and online services share tokenizer/preprocessor/container and return vector, bundle version, input hash, and quality flags. Idempotent batch jobs checkpoint shards and write to versioned columns/index namespaces; online path has rate limits, batching, caching, and SLOs. Monitor norm, NaN, distribution, language, latency, and drift. For migration, create new storage/index, backfill from canonical source under snapshot semantics, validate coverage and retrieval quality, shadow queries against both, then dual-write new changes and reconcile CDC lag. Gradually shift reads, retain rollback, and garbage-collect old versions after retention. Never mix vectors from incompatible spaces.
* **Production Reality & Tradeoffs:** Dual indexes temporarily double cost. Re-embedding can be API/GPU bound and source content may have changed. Approximate-index parameters need retuning for new distributions. Model aliases must not mutate silently.
* **Red Flag:** Replacing the embedding endpoint in place and querying a single index containing old and new vectors.

