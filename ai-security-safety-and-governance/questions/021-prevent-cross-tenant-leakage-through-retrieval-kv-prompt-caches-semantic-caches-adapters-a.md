### Q: Prevent cross-tenant leakage through retrieval, KV/prompt caches, semantic caches, adapters, and telemetry.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Bind every artifact and cache entry to tenant/principal authorization and version; isolate namespaces/credentials; never reuse content/state across scopes without proof of identical access.
* **The Deep Dive:** Retrieval filters before context. KV/prefix caches use tenant salt for private prefixes. Semantic response keys include tenant, ACL, corpus/policy/model versions. Adapter registry and batching enforce tenant authorization/base compatibility. Logs/traces redact and use tenant-scoped RBAC. Permission changes invalidate derived caches/memory. Test confused-deputy and timing/enumeration channels.
* **Production Reality & Tradeoffs:** Physical isolation may be required for high-risk tenants. Shared GPU memory/process bugs are infrastructure risks beyond prompts. Isolation reduces cache efficiency.
Isolation must be end to end: tenant-scoped identities and indexes/adapters, authorization before retrieval, cache keys salted with tenant/model/template/policy, memory allocators that do not expose stale buffers, and telemetry redaction. Semantic caches are especially dangerous because “similar” requests may differ in identity or permissions. Test concurrent adversarial tenants, permission revocation, adapter eviction/reload, prefix collisions, and mixed-version rollback. Fail closed rather than querying a global corpus then filtering output.

* **Red Flag:** Using user ID only in the application database while sharing unscoped vector and semantic caches.
