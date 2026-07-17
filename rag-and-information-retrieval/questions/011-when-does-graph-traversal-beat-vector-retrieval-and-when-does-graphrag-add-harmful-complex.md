### Q: When does graph traversal beat vector retrieval, and when does GraphRAG add harmful complexity?
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Graphs win when answers require explicit entities, typed relations, multi-hop paths, communities, or global aggregation; vector search wins for fuzzy semantic evidence. GraphRAG is valuable only when extraction/link quality and traversal semantics exceed its maintenance cost.
* **The Deep Dive:** A graph represents nodes (entities/documents/events/claims), typed edges, provenance, time, and ACL. Query processing resolves entities, selects seed nodes, traverses constrained paths or communities, ranks subgraphs, and retrieves supporting text. This supports questions like ownership chains, temporal dependencies, and “summarize themes across a corpus,” where nearest chunks lack complete relations. Vector retrieval remains better for paraphrase, unstructured passages, and unknown vocabulary; hybrid graph seeds plus vector evidence is common.

```text
question -> entity/link resolution -> authorized seed nodes -> typed/time-bounded traversal
        -> supporting source spans -> rerank/generate with citations
```

Graph construction via LLM/NLP can invent/merge/split entities and edges; every edge needs source/version/confidence. Traversal can amplify one extraction error, leak through cross-tenant edges, or explode combinatorially. Community summaries become stale lossy derivatives. Benchmark against a strong hybrid baseline on path recall, answer support, update/deletion, latency, and human correction.
* **Production Reality & Tradeoffs:** Graph schema, entity resolution, backfills, and incremental consistency are expensive. Use graphs when relations are first-class and stable, not as fashionable storage. Enforce ACL before traversal and propagate deletion to summaries.
* **Red Flag:** Calling any vector index plus metadata GraphRAG, or trusting LLM-extracted edges without provenance.

