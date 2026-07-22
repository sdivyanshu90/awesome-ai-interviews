### Q: Build simulations, deterministic fixtures, adversarial environments, and replayable agent benchmarks.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Evaluate agents in controlled environments with known state/effects, realistic stochastic simulations, adversarial observations, and replayed production failures. Score both final state and trajectory legality/efficiency.
* **The Deep Dive:** Fixtures provide deterministic accounts, files, APIs, clocks, and injected failures. Simulators model latency, partial failure, conflicting data, permission changes, and user behavior. Adversarial cases include injection, malformed tool outputs, stale memory, duplicate responses, and deceptive success. Record seeds and event traces. Benchmark variants across repeated runs and compare task success, constraint violations, tool accuracy, recovery, steps, cost, and time. Hold out environment templates to reduce overfitting.

  A complete fixture also declares permitted effects, hidden invariants, and a trajectory oracle, so exact effects and forbidden actions are scored separately from text quality. Fault injection must distinguish timeout-before-commit from timeout-after-commit and include partial outages; snapshot model and runtime versions and report variance across repeated runs—one successful trajectory is not evidence of reliability.

* **Production Reality & Tradeoffs:** Simulators encode assumptions and can be gamed. Pair them with human review and canary traffic. Never point an eval agent at real side-effecting systems without isolation.
* **Red Flag:** Using static QA final answers to evaluate an interactive agent.
