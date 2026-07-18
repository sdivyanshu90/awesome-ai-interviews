### Q: Explain chunked prefill, prefill prioritization, decode prioritization, fairness, and starvation.
* **Difficulty:** Principal
* **Category:** Scheduling
* **The 10-Second Pitch:** Chunked prefill divides long prompts into token blocks so decodes can interleave. Scheduler policy balances fast first token for new requests, smooth ITL for active users, throughput, deadlines, and starvation prevention.
* **The Deep Dive:** A monolithic long prefill can block decode and spike ITL. Chunks consume available token budget per iteration; state/KV accumulates until prompt complete. Decode-first protects interactive jitter but can starve prefills under load; prefill-first harms active streams. Use weighted fair queuing, age/deadline boosts, reserved token shares, or service classes. Priority must account for remaining work and KV residency.
* **Production Reality & Tradeoffs:** Chunk size affects GEMM efficiency and scheduler overhead. Preemption may discard/recompute KV. Measure per-class goodput and starvation, not aggregate utilization.
Chunked prefill divides a long prompt into token quanta so decode work can run between chunks. Prefill-first minimizes individual TTFT but can spike active users’ ITL; decode-first protects interactivity but may starve admissions. A scheduler assigns weighted token budgets/deadlines per class, ages waiting work, and caps consecutive chunks. Track time-in-queue separately for prefill and decode, plus deadline misses by length. Fairness is a policy choice, not maximal GPU utilization.

* **Red Flag:** Maximizing throughput with no fairness or deadline model.
