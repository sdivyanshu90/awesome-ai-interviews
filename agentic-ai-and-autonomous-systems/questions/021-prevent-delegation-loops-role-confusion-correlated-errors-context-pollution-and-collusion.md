### Q: Prevent delegation loops, role confusion, correlated errors, context pollution, and collusion.
* **Difficulty:** Principal
* **Category:** Multi-Agent Systems
* **The 10-Second Pitch:** Define a bounded delegation graph, unique task ownership, typed handoff contracts, independent evidence where diversity is intended, and a trusted arbiter/verifier. More agents do not create independent judgment automatically.
* **The Deep Dive:** Each task has an ID, parent, owner, deadline, budget, permitted subdelegation depth, and completion criteria. Reject handoffs that recreate an ancestor task or lack new information. Role prompts do not confer permissions; identities/capabilities do. Share minimal structured state rather than concatenating every transcript. Diversity requires different evidence, models, methods, or randomization; identical agents have correlated failures. An arbiter checks outputs against sources/tests and detects mutually reinforcing unsupported claims.
* **Production Reality & Tradeoffs:** Coordination overhead can dominate. Parallel work needs merge semantics and cancellation. Monitor delegation graph cycles, duplicate work, agreement without evidence, and context growth.
Attach a delegation ancestry chain, maximum depth, unique subtask ID, and parent budget to every message. Supervisors reject tasks already present in the ancestry or semantically duplicate outstanding work. Roles specify allowed data and tools, not personalities. Keep private scratch state isolated and exchange typed claims with provenance. Diversity requires different evidence or methods; sampling multiple near-identical agents does not create independent votes and can amplify one injected observation.

* **Red Flag:** Using majority vote among identical agents as proof.
