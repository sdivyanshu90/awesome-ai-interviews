### Q: Detect corpus, query, embedding, ranking, freshness, and authorization drift.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Version and monitor each stage: source/corpus distribution and age, query mix, embedding geometry, candidate/rank behavior, relevance outcomes, and ACL decisions. Use anchored probes plus production replay to distinguish legitimate content change from broken pipelines.
* **The Deep Dive:** Corpus drift: document counts/tokens/language/domain/source, duplicate rate, parse/OCR failures, age, deletion/ACL lag. Query drift: intents, language, length, entities, no-result and rewrite patterns. Embedding drift: model/version mismatch, norm/NaN, covariance/spectrum, centroid/neighbor stability, quantization, online-offline parity. Ranking drift: score/rank distributions, lexical/vector overlap, candidate source mix, filter selectivity, reranker changes, Recall/nDCG on stable probes. Freshness drift compares required versus retrieved source time. Authorization drift audits pre-retrieval filtering, denied/allowed rates, canary cross-tenant probes, and ACL propagation.

```text
source -> parse/chunk -> embed/index -> query/rewrite -> retrieve/filter -> rerank -> context
 metrics + version + canary at every arrow
```

Maintain immutable anchor queries/documents and rolling privacy-safe replay. When alarms fire, replay with old/new stage artifacts and swap one stage at a time. Outcome labels/citation corrections provide delayed monitoring.
* **Production Reality & Tradeoffs:** Distribution movement may be correct after new content; thresholds need seasonal baselines. Embedding statistics can look stable while relevance collapses. Logging queries risks privacy. Model/index migrations need dual-run comparison and rollback.
* **Red Flag:** Monitoring only vector norms or aggregate answer satisfaction and calling it retrieval drift diagnosis.

