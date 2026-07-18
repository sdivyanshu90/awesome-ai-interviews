### Q: Explain LLM10 Unbounded Consumption, denial of wallet, model DoS, and resource controls.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Attackers can exhaust tokens, GPU, tools, storage, or paid APIs through long inputs, recursive agents, expensive modalities, or adversarial scheduling. Bound resources at admission and runtime.
* **The Deep Dive:** Enforce input/output/context/media limits, per-principal quotas, concurrency, token/cost/time/step/tool budgets, rate limits, queue caps, and cancellation. Estimate KV/cache needs before admission. Detect retry/delegation loops and expensive query fan-out. Apply backpressure/load shedding and circuit breakers. Meter downstream APIs and storage.
* **Production Reality & Tradeoffs:** Limits must account for shared tenants and priority. Very long valid workloads need separate service classes. Cache attacks and partial requests can still consume resources.
Unbounded consumption includes long prompts/outputs, branching agents, decompression/tokenizer bombs, expensive tool fan-out, repeated retries, and adversarial cache misses. Enforce byte and token limits before/after normalization, hierarchical cost budgets, concurrency quotas, deadlines, queue caps, and circuit breakers. Estimate KV/compute before admission and stream with cancellation. Rate limits alone do not stop one authorized request from exhausting GPU memory or spawning costly external operations.

* **Red Flag:** Only rate-limiting requests while allowing unbounded tokens and agent steps.
