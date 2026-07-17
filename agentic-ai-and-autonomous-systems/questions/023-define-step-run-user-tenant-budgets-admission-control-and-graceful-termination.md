### Q: Define step/run/user/tenant budgets, admission control, and graceful termination.
* **Difficulty:** Principal
* **Category:** Reliability
* **The 10-Second Pitch:** Maintain an executor-side budget ledger for tokens, cost, time, steps, retries, tool calls, concurrency, and side effects at nested scopes. Admission predicts resource need; runtime enforces hard limits and returns a useful partial/blocked state.
* **The Deep Dive:** Reserve budget for cleanup, verification, and final reporting. Child tasks inherit capped allocations; unused budget can return to parent. Admission uses request class, context/output bounds, tool risk, queue state, and tenant quota. Per-step checks happen before calls, not after overspend. Termination records completed actions, unresolved goals, evidence, and safe resume token. Rate limits and circuit breakers protect dependencies; high-priority tasks use explicit service classes.
* **Production Reality & Tradeoffs:** Predictions are uncertain, so combine hard ceilings with adaptive replanning. Budget errors must never skip compensation or leave unknown side effects. Expose transparent failure to users.
Budget hierarchically: tenant admission reserves a run ceiling; the run allocates per-stage/child envelopes; each step charges predicted and actual tokens, tool dollars, time, and side-effect risk. Reject work that cannot fit its deadline or minimum safe completion path. Near exhaustion, stop exploration, cancel speculative branches, persist evidence, and return a typed partial result. Do not truncate in the middle of a transaction or after preparing an effect without resolving its status.

* **Red Flag:** Relying on a prompt instruction to stay within budget.
