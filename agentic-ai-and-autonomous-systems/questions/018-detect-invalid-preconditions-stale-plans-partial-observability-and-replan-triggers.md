### Q: Detect invalid preconditions, stale plans, partial observability, and replan triggers.
* **Difficulty:** Principal
* **Category:** Planning
* **The 10-Second Pitch:** Store explicit action preconditions and expected effects, check them immediately before execution, compare observation to expected state, and replan only when material divergence or uncertainty invalidates the current suffix.
* **The Deep Dive:** A plan step contains action, arguments, preconditions, expected postconditions, evidence, timeout, and compensation. Read tools refresh volatile state before writes. Belief state represents known/unknown possibilities under partial observability; information-gathering actions reduce uncertainty. Triggers include failed precondition, unexpected effect, tool/version change, new higher-authority instruction, deadline/budget change, or invalidated dependency. Preserve completed irreversible actions and replan remaining goals rather than restarting blindly.
* **Production Reality & Tradeoffs:** Constant replanning wastes tokens and can oscillate. Debounce transient failures, cap retries, and require monotonic progress. High-risk ambiguous state escalates rather than guessing.
Represent every action with required predicates and the observation version that established them. Immediately before execution, refresh volatile predicates such as balances, locks, deployment versions, or availability. Replan when a precondition fails, a prediction error exceeds threshold, a dependency changes, or remaining expected utility falls below a safe alternative. Do not replan on every cosmetic observation; that causes thrashing. Preserve the invalidated plan and evidence for diagnosis.

* **Red Flag:** Executing an initial plan end-to-end without checking live state.
