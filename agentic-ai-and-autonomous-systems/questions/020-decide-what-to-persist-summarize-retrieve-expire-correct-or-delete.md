### Q: Decide what to persist, summarize, retrieve, expire, correct, or delete.
* **Difficulty:** Principal
* **Category:** Memory
* **The 10-Second Pitch:** Persist authoritative structured state and auditable events; summarize low-value history; retrieve reusable evidence; expire data by purpose; support corrections and propagate deletion. The decision follows task invariants, provenance, privacy, and future utility.
* **The Deep Dive:** Pending actions, approvals, identifiers, and tool outcomes belong in typed state. Event logs preserve chronology and can rebuild state. Semantic memory stores sourced facts, not unsupported model guesses. Summaries are lossy caches with links to original events. TTL differs for transient scratchpads, sessions, profiles, and regulated records. Corrections append provenance and supersede earlier views. Deletion reaches primary stores, vector indexes, caches, summaries, backups, and derived analytics under documented SLA.
* **Production Reality & Tradeoffs:** More memory increases leakage, poisoning, staleness, and retrieval noise. Measure task lift per memory class. Legal holds and audit obligations can conflict with ordinary deletion and need explicit policy.
Use a retention policy by memory class. Raw events are append-only and short-lived; promoted facts require provenance/confidence; summaries expire when their sources change; user preferences require consent and correction; credentials are never memory. Retrieval applies purpose and tenant filters before similarity. Deletion traverses raw events, embeddings, summaries, caches, and learned profiles. Contradictions are preserved as competing claims until an authoritative resolver updates them.

* **Red Flag:** Persisting every token indefinitely because storage is cheap.
