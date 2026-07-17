### Q: Build query routing across lexical, vector, graph, SQL, API, and web-search backends.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Classify the information need and route to the backend whose semantics match it: lexical for exact strings, vector for semantic text, graph for relations, SQL for structured aggregates, APIs for operational state, and web for external freshness.
* **The Deep Dive:** Routing uses deterministic cues plus a model classifier: identifiers/operators, requested aggregation, entities/relations, corpus ownership, freshness, and permissions. It may execute multiple backends in parallel and fuse evidence. Structured queries are generated into a constrained intermediate representation and validated. Every route preserves original query, identity, provenance, timeout, cost, and fallback. A verifier checks whether returned evidence can answer before generation.
* **Production Reality & Tradeoffs:** Wrong routing is a distinct evaluation stage. Web/API results are untrusted and may inject instructions. Parallel fan-out raises cost and tail latency; use confidence and task risk to gate.
* **Red Flag:** Sending every query to every backend or letting the model bypass authorization.

