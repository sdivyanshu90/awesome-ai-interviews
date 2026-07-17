### Q: Compare BM25, dense retrieval, learned sparse retrieval, SPLADE, and late interaction.
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** BM25 gives interpretable lexical saturation; dense retrieval maps query/document to one vector; learned sparse predicts vocabulary impacts; SPLADE is a sparse expansion family; late interaction stores token vectors and matches them at query time for more precision and cost.
* **The Deep Dive:** BM25 sums IDF-weighted term scores with term-frequency saturation and length normalization, excelling at exact names/numbers/rare terms and requiring no neural training. Dense bi-encoders compute one embedding per query/document and ANN search captures paraphrase but can miss exact entities and hides token evidence. Learned sparse models output high-dimensional sparse vocabulary weights; SPLADE-style masked-LM expansion adds related terms and trains with sparsity regularization, retaining inverted-index execution. Late interaction (for example MaxSim) encodes multiple document/query token vectors and sums each query token’s maximum document similarity, preserving fine-grained matching but expanding index/storage/compute.

```text
BM25: query terms -> inverted postings
Dense: query vector -> ANN document vectors
Learned sparse: neural vocabulary weights -> inverted postings
Late interaction: query token vectors -> token-level ANN/MaxSim -> documents
```

Hybrid fusion combines lexical and neural strengths through RRF, calibrated scores, or learned ranking. ACL/filter/freshness apply before or within retrieval.
* **Production Reality & Tradeoffs:** Dense embeddings require versioned backfills; sparse indexes can be large; late interaction has high storage and rerank latency. Compare Recall@k, exact-entity/semantic slices, p99 latency, index bytes, update/deletion, and downstream support.
* **Red Flag:** Calling dense retrieval semantic ground truth or assuming hybrid means averaging incomparable raw scores.

