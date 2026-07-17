### Q: Compare reactive, plan-and-execute, planner-executor, hierarchical, and event-driven agents.
* **Difficulty:** Principal
* **Category:** Planning
* **The 10-Second Pitch:** Reactive agents choose the next action from current state; plan-and-execute creates a plan then acts; hierarchical planners decompose goals; search explores multiple trajectories with a scoring/verification function. Choose based on task horizon, environment uncertainty, and verifier quality.
* **The Deep Dive:** Plans reduce myopic behavior but become stale after observations. Replanning should be triggered by preconditions, failed actions, or new evidence. Classical planning uses explicit states/actions/preconditions/effects; LLMs can propose candidates but symbolic checks should validate hard constraints. Search is valuable only when branching and evaluation are affordable.
Reactive policies minimize planning cost but can thrash; plan-and-execute amortizes reasoning but becomes stale; planner-executor separation permits independent verification; hierarchical plans reduce branching via subgoals; event-driven agents react to asynchronous state. Choose by horizon, observability, tool cost, and reversibility. Hybrid systems plan a few milestones, validate the next preconditions, execute one bounded action, then replan on material observation. Never execute an entire speculative plan without rechecking world state.

* **Production Reality & Tradeoffs:** Log plan revisions and their cause. Long speculative plans waste tokens and can create false certainty; execute cheap information-gathering actions early when they reduce uncertainty.
* **Red Flag:** Asking the model for a 20-step plan once, then executing it blindly.
