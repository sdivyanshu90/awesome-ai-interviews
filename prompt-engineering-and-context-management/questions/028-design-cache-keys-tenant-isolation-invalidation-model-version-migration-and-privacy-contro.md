### Q: Design cache keys, tenant isolation, invalidation, model-version migration, and privacy controls.
* **Difficulty:** Principal
* **Category:** Performance
* **The 10-Second Pitch:** Key caches by every input that changes semantics or authorization, scope physically/logically by tenant and policy, attach dependency versions/freshness, invalidate from change events plus TTL, and migrate with dual namespaces and rollback.
* **The Deep Dive:** For exact response/prefix caches include tenant/security domain, principal class where relevant, normalized token IDs or input digest, model weights/version, adapter, tokenizer/chat template, prompt/examples/tool schemas, decoding, policy, retrieval/index/source versions, locale/time bucket, multimodal digests, and cache dtype/layout/positions. Semantic caches additionally store embedding model, threshold, task type, evidence, and verifier result. Do not key secrets by hashing alone then share across tenants; timing/hit metadata leak.

Maintain dependency graph: source update/deletion, ACL, prompt/policy, tool/schema, model, or index emits invalidation/tombstone; TTL bounds missed events. Encrypt data, minimize cached payload, restrict operators/logs, and audit access. Migration creates new namespace, shadow/dual writes, warms safe prefixes, shifts reads gradually, and retains old rollback; never mix incompatible KV/embedding spaces.

```text
request + execution bundle + tenant/policy + dependency versions -> cache key
change event -> reverse dependency -> invalidate/tombstone
```
* **Production Reality & Tradeoffs:** Complete keys reduce hits; incomplete keys return wrong/unauthorized data. Event invalidation is complex and TTL causes staleness. Measure safe hits, stale/wrong hits, isolation tests, and avoided cost.
* **Red Flag:** Using `hash(prompt)` as a universal key or sharing KV/semantic entries across tenants without compatibility proof.

