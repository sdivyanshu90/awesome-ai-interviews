### Q: Compare prefix/KV prompt caching, provider caching, semantic caching, and response caching.
* **Difficulty:** Senior
* **Category:** Performance
* **The 10-Second Pitch:** Prefix/KV caching reuses exact model computation for identical token prefixes; provider caching is a managed variant; semantic/response caches reuse outputs for equivalent requests. Exactness, invalidation, state, and tenant isolation become stricter as reuse moves from computation to answers.
* **The Deep Dive:** KV prefix reuse requires identical token IDs and compatible model weights, adapter, position encoding/offset, attention/cache dtype/layout, multimodal inputs, and policy scope. Arrange stable system instructions, tool schemas, and shared examples first; put dynamic user/time/nonce content later. Provider caching may use explicit cache objects or automatic prefix detection with TTL/pricing constraints, so version and measure it rather than assume.

Semantic caching embeds a normalized request and retrieves a prior answer, then must verify task, tenant/ACL, locale, source/policy versions, freshness, tool/user state, and similarity. Response caching is safest for deterministic public/versioned queries; never reuse side-effect results or personalized/high-stakes answers without complete state keys.

```text
exact token prefix -> reuse KV compute (same output distribution)
semantic equivalent -> reuse answer only after policy/freshness compatibility
```
* **Production Reality & Tradeoffs:** Dynamic timestamps and reordered JSON destroy exact hits. Loose semantic thresholds return wrong but fluent answers; strict thresholds erase value. Cache timing can leak cross-tenant information. Measure safe hit rate, avoided cost/latency, stale/wrong hits, and fallback.
* **Red Flag:** Keying only by visible prompt text or sharing semantic responses across tenants because embeddings are similar.

