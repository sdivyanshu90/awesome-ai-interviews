### Q: Compare prefix caching, KV reuse, semantic caching, cache salting, and tenant isolation.
* **Difficulty:** Principal
* **Category:** Caching
* **The 10-Second Pitch:** Prefix/KV reuse requires exact token/model state; semantic caching reuses an answer for similar meaning. Salt/scope prevents private cache sharing. Correctness keys include model, tokenizer, prompt/template, adapters, position, and authorization.
* **The Deep Dive:** Exact prefix hash maps token blocks to KV; partial prefix hits reuse longest matching blocks. KV is layer/model/precision/position-specific. Semantic cache embeds normalized queries and returns prior response only under similarity, freshness, and policy validation. Salt includes tenant/principal or public-content scope. ACL, corpus/tool/prompt/model changes invalidate response semantics.
* **Production Reality & Tradeoffs:** Dynamic timestamps or reordered templates destroy exact hits. Semantic similarity can conflate materially different constraints. Side-channel hit timing and cached secrets need consideration.
Prefix caching reuses exact token/template/model state; KV reuse may clone blocks for branches; semantic caching returns a prior answer to a similar query and changes semantics. Exact caches require tokenizer, model, adapter, positional scheme, template, policy, and prefix-token identity in the key. Tenant and authorization scope are mandatory. Salt or partition sensitive domains, use copy-on-write, and invalidate on policy/template changes. A cosine threshold is not a security boundary.

* **Red Flag:** Sharing caches globally because prompts look similar.
