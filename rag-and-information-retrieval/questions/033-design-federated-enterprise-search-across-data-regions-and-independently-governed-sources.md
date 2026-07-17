### Q: Design federated enterprise search across data regions and independently governed sources.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Use a central query coordinator with identity-aware source adapters, but keep data and authorization decisions within governing regions/systems. Merge only authorized result metadata/evidence under explicit residency and freshness rules.
* **The Deep Dive:** The coordinator authenticates, classifies/routs, and sends minimal queries plus delegated scoped identity to regional connectors. Each source enforces ACLs, searches its lexical/vector/index version, and returns provenance, scores, and policy labels. Global fusion handles incomparable scores and partial failures. Content may remain regional; generation can occur in-region or on redacted snippets. Audit traces record source decisions without centralizing forbidden data.
* **Production Reality & Tradeoffs:** Cross-region fan-out drives p99 and egress cost. Define timeout/partial-answer semantics, source health, cache residency, deletion, and legal holds. Never copy all corpora into one convenient vector store.
Score calibration is source-local because BM25, vector, graph, and remote systems have incompatible distributions. The coordinator can use ranks/RRF plus source reliability and freshness, then rerank evidence under a common model. Partial answers list unavailable sources and avoid claiming global completeness. Delegated identity should be audience- and query-scoped; connectors return policy decisions and provenance, not entire inaccessible records for central postfiltering.

* **Red Flag:** Using application-side postfiltering to simulate source-system authorization.
