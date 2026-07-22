### Q: Distinguish an LLM call, chain, workflow, state machine, agent, and autonomous system.
* **Difficulty:** Mid
* **Category:** Architecture
* **The 10-Second Pitch:** A workflow follows a mostly predetermined control graph; an agent selects actions or plans from state under uncertainty. Use workflows for predictable, auditable steps and introduce agentic choice only where adaptive tool selection or recovery creates measurable value.
* **The Deep Dive:** Both can call LLMs and tools. An agent observes state, chooses an action, receives feedback, and loops until termination; a workflow can still have branches, retries, and human approval. The distinction matters because an agent requires explicit action space, state, authority, stop conditions, and trajectory evaluation.
The decisive property is **closed-loop action selection**: observations change subsequent choices. Autonomy is a separate axis describing duration, authority, and human supervision. A state machine may contain one probabilistic decision node yet keep transitions deterministic and auditable.
```text
workflow: input -> fixed graph -> output
agent:    state -> choose action -> observe -> update state -> stop?
```
Use the weakest construct that satisfies adaptation requirements; every additional choice expands the state space to test. A concrete falsification test: rerun the same task with one tool result perturbed; if the executed step sequence cannot change in response, the system is a workflow with LLM calls in it, not an agent, however it is marketed.

* **Production Reality & Tradeoffs:** Unnecessary autonomy increases nondeterminism, attack surface, cost, and debugging burden. A deterministic orchestrator plus narrow LLM decisions is often the best production design.
* **Red Flag:** Calling any application that uses an LLM and an API an autonomous agent.
