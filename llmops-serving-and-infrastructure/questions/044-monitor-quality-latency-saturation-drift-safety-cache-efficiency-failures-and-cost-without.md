### Q: Monitor quality, latency, saturation, drift, safety, cache efficiency, failures, and cost without leaking data.
* **Difficulty:** Principal
* **Category:** Observability
* **The 10-Second Pitch:** Collect stage metrics/traces and privacy-minimized quality signals, with strict access/retention. Link every outcome to versioned bundle and workload slice without logging raw sensitive content by default.
* **The Deep Dive:** Metrics: TTFT/ITL/E2E/goodput, queue/KV/GPU/network, errors/cancel, tokens/cost, cache hit/reuse/eviction, schema/tool success, safety events, and sampled adjudicated quality/drift. Use hashes/IDs, classifications, redaction, encrypted restricted payload sampling, and user consent. Distributed traces carry request/version IDs. Alerts use SLO burn rates and anomaly/slice thresholds.
* **Production Reality & Tradeoffs:** No telemetry means no diagnosis; full prompt logs create a data breach. Sampling can miss rare harm. Establish escalation access and deletion.
Telemetry needs request-class/version labels and privacy-safe payload-derived signals. Track queue, TTFT/ITL/p99, throughput/goodput, GPU/KV/allocator saturation, cache hit/eviction, errors/cancellations, quality/safety samples, drift, and cost per successful task. High-cardinality user/prompt labels leak data and break metrics systems; keep sensitive traces access-controlled with sampling and retention. Join stage spans through opaque IDs, and distinguish model quality drift from traffic-mix and evaluator drift.

* **Red Flag:** Logging every prompt indefinitely for observability.
