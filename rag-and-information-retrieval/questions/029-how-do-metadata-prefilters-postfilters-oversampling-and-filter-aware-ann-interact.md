### Q: How do metadata prefilters, postfilters, oversampling, and filter-aware ANN interact?
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** Prefilters restrict eligible candidates before similarity; postfilters remove results after search; oversampling retrieves extra candidates to survive postfiltering; filter-aware indexes integrate metadata into traversal/partitioning. The choice trades security, recall, and latency.
* **The Deep Dive:** For strict ACLs, eligibility must be enforced before exposure even if implementation searches encrypted/isolated candidates internally. Postfiltering top-k has expected survivors proportional to selectivity and may return none. Oversampling factor must adapt to filter selectivity but can explode work. HNSW graph traversal through ineligible nodes may be safe only if their content/scores are not exposed and storage isolation allows it. Tenant partitions/bitsets/filtered graphs improve predictable recall.

If the eligible fraction is $p$, naively postfiltering $n$ ANN candidates yields roughly $np$ survivors in expectation—but similarity rank and metadata are often correlated, so the realized count can be far lower and must be measured. Adaptive oversampling therefore scales candidate depth by observed selectivity under a hard cap. ACL revocation must invalidate result and semantic caches before previously authorized evidence can be reused.
* **Production Reality & Tradeoffs:** Measure recall and p99 by filter cardinality and combinations. Rapid permission changes require index/cache invalidation. Never broaden filters as a fallback.
* **Red Flag:** Treating metadata filtering as a harmless final SQL WHERE clause.
