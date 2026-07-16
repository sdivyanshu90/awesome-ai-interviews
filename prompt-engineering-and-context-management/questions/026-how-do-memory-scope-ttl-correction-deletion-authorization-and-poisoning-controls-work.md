### Q: How do memory scope, TTL, correction, deletion, authorization, and poisoning controls work?
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Memory is governed data: bind every item to subject/tenant/purpose/provenance, authorize before retrieval, expire by type, append corrections without losing audit, propagate deletion to derivatives, and quarantine untrusted writes.
* **The Deep Dive:** Memory record includes owner/subject, tenant, allowed purposes, source and author, creation/event time, sensitivity, confidence/status, content/version digest, TTL, and deletion lineage. Reads first enforce identity/ACL/purpose/region then rank; post-filtering shared ANN can leak. Writes use an allowlisted schema and policy: user-confirmed preference, verified tool fact, or reviewed consolidation. Retrieved documents/model outputs remain untrusted and cannot self-promote to durable authoritative memory.

Corrections create a new version/tombstone linking superseded values; retrieval uses current valid version while audit preserves history under retention. Deletion traverses source → chunks/embeddings → summaries/semantic caches → backups within declared SLO and verifies absence. TTL differs: transient task state minutes/days, events per retention, stable preference until changed/consent revoked. Poisoning detection watches unusual write volume, instruction-like content, provenance, conflicts, and cross-user references; quarantine and revoke.
* **Production Reality & Tradeoffs:** Strong isolation and deletion reduce cache/index efficiency. Immutable audit conflicts with erasure—store minimal legal proof, not deleted content. Approximate search plus ACL is hard; separate namespaces may be necessary.
* **Red Flag:** Authorizing memory after retrieval or allowing the agent to store any text it finds as a trusted fact.

