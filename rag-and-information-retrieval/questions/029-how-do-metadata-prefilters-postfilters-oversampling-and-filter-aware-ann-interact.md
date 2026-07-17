### Q: How do metadata prefilters, postfilters, oversampling, and filter-aware ANN interact?
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** Prefilters restrict eligible candidates before similarity; postfilters remove results after search; oversampling retrieves extra candidates to survive postfiltering; filter-aware indexes integrate metadata into traversal/partitioning. The choice trades security, recall, and latency.
* **The Deep Dive:** For strict ACLs, eligibility must be enforced before exposure even if implementation searches encrypted/isolated candidates internally. Postfiltering top-k has expected survivors proportional to selectivity and may return none. Oversampling factor must adapt to filter selectivity but can explode work. HNSW graph traversal through ineligible nodes may be safe only if their content/scores are not exposed and storage isolation allows it. Tenant partitions/bitsets/filtered graphs improve predictable recall.
* **Production Reality & Tradeoffs:** Measure recall and p99 by filter cardinality and combinations. Rapid permission changes require index/cache invalidation. Never broaden filters as a fallback.
If eligible fraction is $p$, naive postfiltering $n$ candidates yields roughly $np$ survivors in expectation, but ANN ranking and metadata are correlated, so this is not a guarantee. Adaptive oversampling uses observed selectivity with a cap; filter-aware traversal uses bitsets/partitions or specialized graphs. Always distinguish nodes traversed internally from content exposed externally. ACL revocation must invalidate result and semantic caches before stale authorized evidence is reused.

* **Red Flag:** Treating metadata filtering as a harmless final SQL WHERE clause.
