### Q: Compare symmetric versus asymmetric, single-vector versus multi-vector, and domain-specific embeddings.
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** Symmetric embeddings serve interchangeable text similarity; asymmetric retrieval distinguishes query and passage roles. Single vectors scale cheaply but compress evidence; multi-vector preserves token/region interactions at storage/latency cost. Domain tuning helps only with representative labels and retention tests.
* **The Deep Dive:** Symmetric encoders/objectives place both sides in one role and support clustering/duplicate similarity. Asymmetric systems use separate encoders or role prefixes such as query/passage, reflecting short intent versus long evidence; swapping roles changes scores. Single-vector bi-encoders precompute one $D$-vector/document and use ANN. Multi-vector models store token/segment vectors and use late interaction such as $\sum_i\max_jq_i^Td_j$, improving exact entity and compositional matching but multiplying index size.

Domain-specific embeddings may continue pretrain or contrastively tune on clicks/judgments/hard negatives. They risk overfitting jargon/templates, collapsing general/multilingual alignment, and encoding biased feedback. Evaluate zero-shot/general retention and cross-domain queries. For long documents, hierarchical or parent-child vectors provide intermediate trade-offs.

```text
single: query vector <-> document vector
multi:  each query token -> best document token/segment -> aggregate
```
* **Production Reality & Tradeoffs:** Asymmetric format mistakes silently hurt. Multi-vector indexes complicate updates/quantization. Domain model/version migration requires full re-embedding and dual-index rollout. Compare quality per index byte and p99 latency.
* **Red Flag:** Using a query-prefix model without prefixes or claiming multi-vector is always more semantic.

