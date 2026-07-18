### Q: Compare colocated versus disaggregated prefill/decode and KV transfer architectures.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Colocation avoids KV transfer and shares GPUs; disaggregation specializes/scales prefill and decode independently but must move large KV state reliably and schedule both stages.
* **The Deep Dive:** In colocation, one replica admits a request, runs compute-heavy prefill, retains its local KV blocks, and performs bandwidth-heavy decode. There is no handoff or serialization, prefix locality is simple, and spare capacity is fungible; however long prefills interfere with active decodes and the two phases cannot be scaled or tuned independently. Disaggregation creates specialized prefill and decode pools. The coordinator must select compatible parallel layouts, reserve decode KV/concurrency before expensive prefill, transfer KV blocks plus token/position/rope/cache metadata, publish ownership atomically, then start decoding.

```text
colocated:     request -> [ GPU: prefill | local KV | decode ] -> stream

disaggregated: request -> prefill pool -> KV transfer fabric -> decode pool -> stream
                              |                  |                  |
                         compute tuned      backpressure       bandwidth tuned
                                               ^
                                      decode capacity reserved first
```

  Compatibility includes exact model/adapters, KV dtype/layout, layer partition, tensor-parallel rank mapping, position encoding, and block size. Direct GPU transfer/RDMA avoids host staging where supported. Failure semantics need a request ID and ownership state: before publish, prefill may retry; after publish, duplicate decode must be prevented; lost KV may be retransferred or recomputed. Prefix-aware routing can bypass prefill when the decode side already owns a compatible prefix.
* **Production Reality & Tradeoffs:** Disaggregation helps workloads with large/skewed prompts or phase interference, but short prompts can lose to queueing plus transfer. KV transfer grows with layers, context, heads, and dtype and competes for fabric with tensor parallelism. Independently autoscale both pools with backpressure; otherwise prefill creates unusable work faster than decode drains it. Compare p50/p99 TTFT, inter-token latency, handoff time, transfer bandwidth, recompute rate, queue depth, and cost at real length distributions.
* **Red Flag:** Ignoring KV transfer and decode reservation when proposing disaggregation.
