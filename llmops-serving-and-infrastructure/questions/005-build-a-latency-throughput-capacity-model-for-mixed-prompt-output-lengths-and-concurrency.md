### Q: Build a latency-throughput-capacity model for mixed prompt/output lengths and concurrency.
* **Difficulty:** Principal
* **Category:** Capacity Planning
* **The 10-Second Pitch:** Segment workload by prompt/output/model/SLO, estimate prefill and decode token service demands plus KV residency, then validate a queueing simulation against measured kernels and tails.
* **The Deep Dive:** For class $i$, arrival $λ_i$, input $P_i$, output $O_i$. Prefill work scales near $P_i$ plus attention effects; decode performs $O_i$ iterations with context increasing. KV occupancy follows active sequences and length. Scheduler batches compatible tokens; Little's Law relates concurrency, arrival, and residence time. Include tokenization, network, cache hits, tool delays, and fragmentation. Simulate bursts/cancellations/priority and derive GPUs for utilization below tail-instability point.
* **Production Reality & Tradeoffs:** Means underpredict heavy tails. Kernel throughput depends on batch/shape and interference. Recalibrate from production traces and reserve headroom/failure capacity.
Model a request by prompt $P$, output $O$, and service demand for prefill/decode. Capacity is constrained simultaneously by compute, weight/KV bandwidth, KV memory, and scheduler slots. Use production joint distributions rather than mean lengths; a few long prompts can dominate TTFT and KV occupancy. Validate the model with load tests sweeping concurrency until the SLO knee, then include headroom for failures, fragmentation, warmup, and skew. Queueing delay diverges near saturation.

* **Red Flag:** Dividing peak benchmark tokens/s by average tokens/request.
