### Q: Explain LangGraph-style nodes, conditional edges, reducers, checkpoints, interrupts, and durable execution.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Model an agent as a graph of explicit state transitions: nodes perform deterministic code, model calls, or tools; edges encode routing; reducers merge state; checkpoints make execution resumable. LangGraph operationalizes this pattern but the design principle is framework-independent.
* **The Deep Dive:** Typed state prevents accidental prompt-only memory. Conditional edges route on validated results, not arbitrary prose. Checkpoints allow interruption for approval, retry after failure, replay, and durable long-running tasks. Reducers make parallel branches’ merge semantics explicit, avoiding silent last-write-wins bugs.
Nodes should be small deterministic or model-backed functions over a versioned state schema; conditional edges inspect typed results, and reducers define how concurrent updates merge. Checkpoints are committed at side-effect boundaries with run/node attempt IDs. Interrupts persist a resumable approval request rather than blocking a worker. Durable execution requires idempotent nodes, schema migrations, replay semantics, and explicit compensation; a graph library does not supply those application contracts automatically.

* **Production Reality & Tradeoffs:** Version graph definitions and state schemas. Durable execution needs migrations and exactly-once/at-least-once reasoning for tool effects.
* **Red Flag:** Treating an agent loop with mutable global chat history as a reliable workflow engine.
