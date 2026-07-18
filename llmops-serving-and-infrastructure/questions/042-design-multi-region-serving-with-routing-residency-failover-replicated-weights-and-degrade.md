### Q: Design multi-region serving with routing, residency, failover, replicated weights, and degraded modes.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Route users to compliant healthy regions, replicate immutable models/configs, keep sensitive state region-scoped, and define failover that respects residency and capacity. Degrade safely when cross-region transfer is forbidden.
* **The Deep Dive:** Global gateway uses identity, residency policy, latency, model availability, and load. Artifact registry distributes signed weights/images; deployment pins versions. Conversation/KV/cache may remain regional; durable user memory uses approved replication. Health includes quality/dependency, not just liveness. Failover prewarms capacity and may drop private cache, shorten context, route smaller model, or return retry rather than violate policy.
* **Production Reality & Tradeoffs:** GPU failover capacity is expensive and cold starts long. Active-active version skew complicates cache/results. Test regional loss and data-plane partitions.
Route by residency, model availability, capacity, latency, and health while keeping tenant data in allowed regions. Replicate immutable weights and policies; region-specific caches and logs obey locality. Failover needs preprovisioned capacity and compatible model/template/tokenizer versions, or it creates an overload cascade. Define degraded modes for unavailable tools/indexes and idempotency across regional retries. Regularly evacuate a region under load and measure lost/in-flight request semantics, not only DNS recovery.

* **Red Flag:** Failing over regulated prompts to any available region.
