### Q: Compare exact search, IVF, PQ/OPQ, HNSW, DiskANN, ScaNN, and GPU indexes.
* **Difficulty:** Senior
* **Category:** Systems
* **The 10-Second Pitch:** Exact search maximizes recall at linear cost. IVF prunes coarse clusters; PQ compresses vectors; HNSW traverses a proximity graph; DiskANN is SSD-aware; ScaNN combines partitioning/quantization; GPU indexes exploit high-throughput parallel distance computation.
* **The Deep Dive:** IVF probes selected centroids (`nprobe`) then scans their postings. PQ stores subvector codebook IDs and approximate lookup distances; OPQ rotates data first. HNSW has high recall and RAM/link overhead. DiskANN builds graph/search layouts optimized for SSD reads and cached neighborhoods. GPU brute force can beat complex indexes at moderate corpus or large batch, but transfer and memory matter. Hybrid indexes commonly re-score candidates using full vectors.
* **Production Reality & Tradeoffs:** Compare recall-latency-memory-build-update curves under filters and concurrency. Index choice depends on corpus size, mutation rate, batch, hardware, and durability—not a universal leaderboard.
IVF search cost grows with probed postings; PQ trades vector bytes for quantization error; HNSW trades RAM/build complexity for graph traversal; disk-oriented graphs trade random SSD reads and caching; GPU brute force trades HBM capacity for enormous batched bandwidth. Plot Pareto frontiers at the same recall, filter selectivity, update rate, and concurrency. A benchmark on static unfiltered vectors does not predict a multi-tenant mutable production index.

* **Red Flag:** Selecting HNSW because it is popular without capacity or update analysis.
