### Q: Prevent infinite loops, repeated tool calls, runaway spend, and non-terminating plans.
* **Difficulty:** Senior
* **Category:** Reliability
* **The 10-Second Pitch:** Enforce per-run step, time, tool-call, token, cost, and retry budgets; detect repeated state-action pairs; require monotonic progress or human escalation; and make every node return an explicit terminal, retryable, or failed state.
* **The Deep Dive:** Hash normalized state plus action to detect loops, but allow bounded legitimate polling. Maintain a budget ledger independent of model claims. A planner should validate that an action changes state or gathers new information; otherwise terminate with an actionable failure report. Circuit breakers disable failing tools.
Termination is enforced outside the model with maximum steps, wall time, tokens, tool calls, monetary cost, repeated-state detection, and per-tool quotas. Hash a canonicalized state/action pair to identify cycles; detect progress through task-specific monotonic invariants, not prose variation. On exhaustion, cancel outstanding calls, preserve checkpoint/evidence, avoid new irreversible actions, and return a typed incomplete result. Budgets must cover nested delegation so child agents cannot reset the parent limit.

* **Production Reality & Tradeoffs:** Budget exhaustion is a normal product outcome, not an exception to hide. Tell users what was attempted and what approval/input is needed next.
* **Red Flag:** Relying on “stop when done” in the system prompt.
