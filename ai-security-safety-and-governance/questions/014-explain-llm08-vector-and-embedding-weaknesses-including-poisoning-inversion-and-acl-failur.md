### Q: Explain LLM08 Vector and Embedding Weaknesses including poisoning, inversion, and ACL failure.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Embedding pipelines can ingest malicious content, leak semantic information, retrieve across tenants, or apply filters after exposure. Secure ingestion, namespaces/ACLs, provenance, filtering, and index lifecycle.
* **The Deep Dive:** Validate source ownership and content; bind vector to tenant/document/version/ACL; enforce eligibility before returning context; isolate high-risk tenants; protect embedding APIs against inversion/enumeration; rate limit queries; propagate deletes/permission changes to indexes/caches. Detect anomalous clusters/hubs and authority manipulation.
* **Production Reality & Tradeoffs:** Vectors are not anonymization—similarity and inversion can reveal content. ANN filters can reduce recall; security still fails closed. Re-embedding migrations must preserve ACL metadata.
Embedding risks include malicious neighbors, membership/inversion leakage, cross-tenant mixing, unauthorized metadata, and postfilter recall/security bugs. Enforce ACLs before evidence exposure, partition or cryptographically scope tenants, authenticate ingestion, preserve provenance, and detect anomalous clusters/query patterns. Embeddings are personal/sensitive data when they encode such content. Never use similarity as authorization, and include index/embedding version in cache keys and audit traces.

* **Red Flag:** Using a global vector index and asking the LLM to ignore unauthorized chunks.
