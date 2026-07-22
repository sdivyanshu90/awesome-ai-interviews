### Q: Design federated enterprise search across data regions and independently governed sources.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Use a central query coordinator with identity-aware source adapters, but keep data and authorization decisions within governing regions/systems. Merge only authorized result metadata/evidence under explicit residency and freshness rules.
* **The Deep Dive:** The coordinator authenticates, classifies/routes, and sends minimal queries plus delegated scoped identity to regional connectors. Each source enforces ACLs, searches its lexical/vector/index version, and returns provenance, scores, and policy labels. Global fusion handles incomparable scores and partial failures. Content may remain regional; generation can occur in-region or on redacted snippets. Audit traces record source decisions without centralizing forbidden data.

Score calibration is source-local: BM25, vector, graph, and remote APIs produce incompatible score distributions, so the coordinator fuses ranks (for example with RRF) weighted by source reliability and freshness, then reranks pooled evidence under one common model. Partial answers must name the unavailable sources rather than imply global completeness. Delegated identity should be audience- and query-scoped so each connector enforces policy locally instead of shipping inaccessible records for central postfiltering.
* **Production Reality & Tradeoffs:** Cross-region fan-out drives p99 and egress cost. Define timeout/partial-answer semantics, source health, cache residency, deletion, and legal holds. Never copy all corpora into one convenient vector store.
* **Red Flag:** Using application-side postfiltering to simulate source-system authorization.
