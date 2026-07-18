### Q: Design admission control using KV capacity, token budgets, deadlines, and service classes.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Predict each request's worst/expected KV and compute demand, admit only if its class SLO can be met, reserve headroom, and enforce runtime token/time/cost caps.
* **The Deep Dive:** Inputs include model, prompt tokens, max output, beam/speculative width, cache hit, priority, deadline, and tenant quota. Capacity model considers free blocks, active growth, prefill token budget, queue delay, and failure reserve. Admission can reject, queue, route, shorten limits with consent, or choose smaller model. Runtime reconciles estimates and cancels/evicts safely.
* **Production Reality & Tradeoffs:** Output length is uncertain; pessimism wastes capacity and optimism causes OOM/tails. Service classes need anti-abuse and fairness. Fail before allocating expensive state.
Admission should reserve predicted KV blocks and a conservative compute/deadline envelope before accepting. Inputs include current free blocks, fragmentation reserve, active decode growth, prompt length, requested maximum output, service class, and cancellation history. Reject, queue, or downgrade before allocating expensive state. Predictions are uncertain, so update reservations as tokens complete and retain emergency headroom; allowing every request to start and relying on OOM recovery creates correlated tail failures.

* **Red Flag:** Admitting every request and relying on CUDA OOM handling.
