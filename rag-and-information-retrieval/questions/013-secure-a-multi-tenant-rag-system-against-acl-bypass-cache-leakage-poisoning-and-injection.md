### Q: Secure a multi-tenant RAG system against ACL bypass, cache leakage, poisoning, and injection.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Bind identity/tenant/purpose to ingestion, index, retrieval, cache, and generation; enforce ACL before candidate content leaves storage; carry provenance; isolate caches; and keep model/tool authority independent from retrieved text.
* **The Deep Dive:** Authenticate connectors and users with signed tenant/principal claims. At ingestion preserve source ACL/version and reject/quarantine untrusted updates. Strong isolation uses tenant namespaces/keys/indexes; a shared index requires pre-filtered ANN or secure candidate generation—post-filtering leaked candidates is unsafe and hurts recall. Rerank/context only authorized text. Prefix/semantic/response caches key tenant, policy, model, source/index versions; adapters/memory/logs are likewise scoped. Canary documents test cross-tenant retrieval.

Retrieved content is untrusted and may inject instructions. Envelope with provenance, render as evidence, and expose no broad credentials. Generated tool proposals pass independent principal/resource authorization, semantic validation, egress/DLP, budgets, and bound approval. Poisoning controls include signed source lineage, write-rate/anomaly detection, embedding/outlier/duplicate analysis, reviewer workflow, and rapid provenance-based quarantine/purge.

```text
identity -> ACL-aware retrieval -> authorized untrusted evidence -> model proposal
                                                     -> deterministic policy -> scoped executor
```
* **Production Reality & Tradeoffs:** Strict isolation reduces shared-cache/index efficiency. ACL changes/deletion must invalidate derivatives quickly. Injection classifiers do not replace least privilege. Audit logs need minimization and regional controls.
* **Red Flag:** Retrieving globally then asking the model not to reveal unauthorized chunks.

