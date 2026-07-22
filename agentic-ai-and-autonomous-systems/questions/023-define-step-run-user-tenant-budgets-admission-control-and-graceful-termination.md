### Q: Define step/run/user/tenant budgets, admission control, and graceful termination.
* **Difficulty:** Principal
* **Category:** Reliability
* **The 10-Second Pitch:** Maintain an executor-side budget ledger for tokens, cost, time, steps, retries, tool calls, concurrency, and side effects at nested scopes. Admission predicts resource need; runtime enforces hard limits and returns a useful partial/blocked state.
* **The Deep Dive:** Reserve budget for cleanup, verification, and final reporting. Child tasks inherit capped allocations; unused budget can return to parent. Admission uses request class, context/output bounds, tool risk, queue state, and tenant quota. Per-step checks happen before calls, not after overspend. Termination records completed actions, unresolved goals, evidence, and safe resume token. Rate limits and circuit breakers protect dependencies; high-priority tasks use explicit service classes.

  Each step charges predicted cost before the call and actual cost after; reject work that cannot fit its deadline or a minimum safe completion path, and near exhaustion stop exploration, cancel speculative branches, and return a typed partial result rather than truncating mid-transaction or leaving a prepared effect unresolved. Failure trace for a step-only budget: a run whose every step stays under its per-step cap can retry a permanently failing tool forever, so a test that drives such a tool must observe termination from the run-level ledger, not from step counts alone.

* **Production Reality & Tradeoffs:** Predictions are uncertain, so combine hard ceilings with adaptive replanning. Budget errors must never skip compensation or leave unknown side effects. Expose transparent failure to users.
* **Red Flag:** Relying on a prompt instruction to stay within budget.
