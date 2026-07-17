### Q: Compare query rewriting, expansion, decomposition, HyDE, multi-query, and step-back prompting.
* **Difficulty:** Senior
* **Category:** RAG
* **The 10-Second Pitch:** These methods transform an underspecified query for retrieval: rewriting clarifies, expansion adds terms, decomposition splits facets/hops, HyDE embeds a hypothetical answer, multi-query diversifies formulations, and step-back retrieves broader principles. Each can drift and must preserve original constraints.
* **The Deep Dive:** A rewrite resolves references, spelling, entities, dates, and conversation context into a standalone query. Expansion adds synonyms/acronyms/lexical terms, useful for BM25. Decomposition creates subqueries whose evidence sets are joined and is necessary for multi-hop questions; define stop/merge logic. HyDE asks a model for a hypothetical relevant passage then embeds it, moving the query toward document style but risking fabricated specificity. Multi-query generates diverse rewrites and fuses results, improving recall at request/cost/dedup expense. Step-back abstracts to a higher-level concept, retrieves principles, then combines with specific evidence.

Always retain the original query and immutable constraints. Validate generated query for tenant scope, time, entity, and intent; never let a rewrite broaden authorization. Run lexical/dense backends as appropriate and fuse by ranks/calibration. Log transformations/provenance and diagnose whether misses arise from rewriting, index, or ranking.
* **Production Reality & Tradeoffs:** More queries raise latency and false positives; parallelism helps but burdens stores. Query models can be injected through conversation. Gate transformations by intent and use cheap baseline first. Evaluate recall gain versus context precision/cost.
* **Red Flag:** Replacing the original query with one generated rewrite and trusting its invented entities.

