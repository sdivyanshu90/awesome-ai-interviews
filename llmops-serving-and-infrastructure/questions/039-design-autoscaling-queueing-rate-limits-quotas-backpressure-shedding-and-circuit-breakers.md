### Q: Design autoscaling, queueing, rate limits, quotas, backpressure, shedding, and circuit breakers.
* **Difficulty:** Principal
* **Category:** Platform
* **The 10-Second Pitch:** Scale from token/KV-aware demand and SLO, not CPU. Bound queues and per-tenant use, propagate backpressure, shed low-priority work before collapse, and isolate failing dependencies with circuit breakers.
* **The Deep Dive:** Signals include queued prefill/decode tokens, KV occupancy, goodput, TTFT/ITL, active sequences, and cold-start lead time. Predictive/warm scaling handles long startup. Token buckets enforce request+token quotas. Bounded priority queues apply deadlines. Backpressure reaches gateways/agents; shedding rejects, downgrades model/context, or defers batch jobs. Circuit breakers stop retry storms and expose fallback.
* **Production Reality & Tradeoffs:** Autoscaling reacts slower than bursts; reserve headroom. Aggressive retries amplify overload. Fairness and paid tiers need explicit policy.
Autoscale on predicted work and queue delay, not request count alone. Admission and quotas reserve token/KV budgets; bounded queues expose overload; backpressure propagates to callers; shedding rejects low-priority or impossible-deadline work; circuit breakers stop retry amplification when dependencies fail. GPU startup delay makes reactive scaling insufficient, so use warm capacity and forecasted load. Validate overload with flash crowds, long-context adversaries, regional loss, and cancellation.

* **Red Flag:** Scaling on average GPU utilization alone.
