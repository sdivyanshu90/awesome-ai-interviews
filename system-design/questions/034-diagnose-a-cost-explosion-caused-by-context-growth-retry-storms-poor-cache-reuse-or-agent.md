### Q: Diagnose a cost explosion caused by context growth, retry storms, poor cache reuse, or agent loops.
* **Difficulty:** Principal
* **Category:** Cost Debugging
* **The 10-Second Pitch:** Reconcile cost by tenant/route/stage and decompose tokens, attempts, cache eligibility/hits, tool steps, GPU time/KV occupancy, then stop amplification with budgets, idempotency, breakers, bounded context, and loop termination.
* **The Deep Dive:** Start from billing and infrastructure totals and allocate by request/tenant/model. Compare distributions of input/output tokens, conversation/context length, retrieved chunks, summary frequency, cache lookups/hits/invalidations, attempts/retry reason, tool/model steps, cancellations, GPU-seconds, and idle capacity before/after. Trace amplification chains: a timeout after commit triggers retry; cache key changed with prompt version; dynamic timestamps destroy prefixes; memory appends full history; an agent treats tool error as reason to replan forever. Check provider 429/5xx, client retries, gateway retries, and worker retries for multiplication. Contain with per-task token/tool/cost/time budgets, max steps, no-progress detection, idempotency, global deadlines, retry budgets/jitter, circuit breakers, context compaction, stable cacheable prefixes, and cost anomaly alerts.
* **Production Reality & Tradeoffs:** Aggressive truncation/cache can hurt quality or freshness; disabling retries harms availability. Optimize cost per successful task and preserve evidence for incidents. Attribute shared reserved costs separately from marginal usage.
```text
cost/request
  = prompt tokens + output tokens + retrieval/tool calls
  + retries + agent branches + reserved/idle infrastructure
            │
            ├─ segment by route, tenant, task, model, cache hit
            └─ normalize by successful task, not raw request
```
A causal diagnosis changes one factor at a time using replay: cap context, disable retries/branches, force cache hits, or pin routing, then reconcile metered usage with provider/GPU invoices.

* **Red Flag:** Negotiating a lower token price while leaving recursive retries, unbounded context, and agent loops intact.
