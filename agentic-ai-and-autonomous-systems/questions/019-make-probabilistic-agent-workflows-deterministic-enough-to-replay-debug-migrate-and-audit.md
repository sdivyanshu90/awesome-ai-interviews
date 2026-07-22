### Q: Make probabilistic agent workflows deterministic enough to replay, debug, migrate, and audit.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Put nondeterminism behind versioned interfaces and persist all inputs, outputs, state transitions, tool results, policy decisions, and random/model settings. Replay with recorded observations or sandboxed fakes so side effects are never repeated.
* **The Deep Dive:** Use a durable state machine with immutable events and idempotent reducers. Record model/provider snapshot, rendered messages, tokenizer, sampling, tool/schema versions, retrieval IDs, time-dependent inputs, and approval digests. Checkpoint after externally visible transitions. A replay mode substitutes recorded tool outputs and clocks; a re-evaluation mode deliberately calls new models against the same state. Schema migrations transform versioned state with tests.

  Serialize state canonically and record seeds where the provider supports them. Exact replay feeds recorded observations back without performing effects; semantic replay permits a new model but asserts transition invariants, with event IDs, attempt IDs, and idempotency keys disambiguating retries. A state-schema migration must be pure and tested on historical checkpoints before old runs resume.

* **Production Reality & Tradeoffs:** Exact model outputs may remain nondeterministic across hardware/provider changes; auditability means attributable inputs/effects, not necessarily bitwise reproduction. Logs require minimization and access control.
* **Red Flag:** Logging only the final answer and calling the agent reproducible.
