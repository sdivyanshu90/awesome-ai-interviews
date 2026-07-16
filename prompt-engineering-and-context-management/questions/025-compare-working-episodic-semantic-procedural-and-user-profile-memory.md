### Q: Compare working, episodic, semantic, procedural, and user-profile memory.
* **Difficulty:** Principal
* **Category:** Context Management
* **The 10-Second Pitch:** Working memory is current run state; episodic memory is timestamped events; semantic memory is consolidated knowledge; procedural memory is approved workflows; profile memory is durable user preference. They differ in authority, retrieval, update, retention, and privacy.
* **The Deep Dive:** Working memory contains current goal, plan, observations, pending actions, and budgets; keep it structured and checkpointed, not blindly long-term. Episodic memory records what happened with time/source/run and is useful for replay or similar-case retrieval but can be noisy. Semantic memory consolidates stable facts with evidence/version/confidence and needs conflict/correction. Procedural memory is versioned instructions/runbooks/tool schemas—higher governance, not model-written habit. User profile stores explicitly consented stable preferences and accessibility needs, scoped to identity/tenant and never inferred sensitive traits without policy.

```text
current run -> working state
completed run -> episodic events -- consolidation/review --> semantic facts
approved organization process ---------------------------> procedural memory
explicit user preference -------------------------------> profile memory
```

Retrieval filters by principal, purpose, time, provenance, sensitivity, and relevance. Tool output/model guesses do not automatically become facts. Each type has TTL, correction, deletion, audit, and authoritative-source precedence.
* **Production Reality & Tradeoffs:** More memory increases personalization but also poisoning, stale-state, privacy, and context risks. Consolidation can hallucinate. Separate storage/indexes/policies where authority differs and measure whether memory improves task success.
* **Red Flag:** Putting every conversation into one shared vector database and calling it long-term memory.

