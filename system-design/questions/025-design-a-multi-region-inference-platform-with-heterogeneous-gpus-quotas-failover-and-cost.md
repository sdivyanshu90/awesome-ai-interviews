### Q: Design a multi-region inference platform with heterogeneous GPUs, quotas, failover, and cost control.
* **Difficulty:** Principal
* **Category:** Infrastructure
* **The 10-Second Pitch:** Route by residency, model capability, SLO, topology, live KV/queue capacity, and tenant quota; maintain signed warm bundles, regional autonomy, tested failover, and workload-aware placement across GPU classes.
* **The Deep Dive:** Global control plane manages immutable model/runtime/tokenizer/prompt/policy bundles, placement intent, quotas, and rollout, while each region’s data plane can continue during control-plane loss. Gateway resolves residency and tenant allowlists before latency/cost routing. Model pools declare GPU SKU, memory, parallel layout, quantization, max context, and benchmarked goodput. Admission budgets tokens and KV; queues isolate interactive, long-context, batch, and priority classes. Autoscaling considers queue delay, predicted work, KV pressure, cold-start time, and error budget. Replicate weights/artifacts with digest verification and warm pools; prefix caches normally remain regional/tenant-scoped. Failover reserves capacity, preserves request idempotency, and selects degraded model/async response if compatible. Chargeback covers tokens, KV occupancy, adapter residency, and dedicated headroom.
* **Production Reality & Tradeoffs:** Heterogeneous fleets complicate kernels and capacity normalization. Failover traffic can overload the survivor. Residency may prohibit cross-region routing. Strict p99 needs expensive headroom; spot capacity fits checkpointable batch, not critical streams.
```text
global router
  ├─ region A: residency policy -> admission -> warm GPU pools
  ├─ region B: residency policy -> admission -> warm GPU pools
  └─ failover: only compatible bundle + reserved capacity
        model/tokenizer/template/policy/adapter versions
```
A region is ready only when artifact compatibility, KV/runtime warmup, quotas, and downstream dependencies have been exercised.

* **Red Flag:** DNS-routing to the nearest region and assuming every GPU/runtime/model replica is interchangeable.
