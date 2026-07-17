### Q: Train and evaluate a reranker with click bias, hard negatives, long documents, and latency constraints.
* **Difficulty:** Principal
* **Category:** Retrieval
* **The 10-Second Pitch:** Train on graded query-document relevance with difficult retrieved negatives, correct logged-feedback bias, and preserve enough document context. Evaluate ranking lift and answer support at the candidate count allowed by the latency budget.
* **The Deep Dive:** Cross-encoders jointly encode query/document and can learn negation and phrase interaction. Training data combines judged positives, random negatives, BM25/dense hard negatives, and near-miss false-negative review. Clicks are censored by previous rank and UI; use randomized exploration, inverse propensity, or debiased objectives. Long documents require passage selection, hierarchical scoring, or late chunking. Evaluate nDCG/MRR/Recall after rerank and end-to-end citations, then profile batching at `k`.
* **Production Reality & Tradeoffs:** Reranker gains saturate and p99 grows with candidate length/count. Distribution shifts when upstream retrieval changes. Store model, candidate-generation, and label-policy versions together.
* **Red Flag:** Training only on easy random negatives or evaluating candidates the reranker never sees in production.

