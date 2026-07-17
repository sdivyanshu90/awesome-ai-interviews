### Q: Explain product quantization codebooks, residual quantization, recall loss, and re-scoring.
* **Difficulty:** Principal
* **Category:** Math
* **The 10-Second Pitch:** PQ splits a vector into subspaces and stores the nearest centroid ID per subspace. Approximate distance is a sum of query-to-codeword lookup distances; residual and multi-stage schemes quantize remaining error. Full-vector re-scoring recovers precision for a small candidate set.
* **The Deep Dive:** For dimension `D`, split into `m` subvectors and learn `k` centroids each. A code uses `m log2(k)` bits. At query time, build `m×k` distance tables and sum indexed entries. OPQ learns a rotation to distribute variance across subspaces. Residual quantization encodes `x-c1` in later codebooks, increasing fidelity and compute. Quantization can change neighbor order, especially near decision boundaries; re-score top `R>k` exact vectors.
* **Production Reality & Tradeoffs:** Training codebooks on stale/unrepresentative data harms recall. Compression affects index build, updates, and memory bandwidth. Validate by query slice and filters, not only reconstruction MSE.
For $m$ subspaces and $k=256$ centroids, a vector uses $m$ bytes plus metadata. Asymmetric distance computation compares an unquantized query with database codewords using $m$ lookup tables. Residual stages reduce remaining error but add dependent lookups. Evaluate neighbor-rank swaps directly—reconstruction MSE weights every direction, while retrieval depends on local margins and query distribution. Exact re-scoring requires retaining full vectors or a higher-fidelity store.

* **Red Flag:** Equating low reconstruction error with guaranteed nearest-neighbor recall.
