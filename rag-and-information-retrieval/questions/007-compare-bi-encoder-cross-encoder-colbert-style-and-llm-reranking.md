### Q: Compare bi-encoder, cross-encoder, ColBERT-style, and LLM reranking.
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** Bi-encoders precompute document vectors for scalable first-stage search; cross-encoders jointly score query-document tokens for precision; late interaction stores token vectors between them; LLM rerankers add flexible reasoning at highest cost and instability.
* **The Deep Dive:** A bi-encoder computes $q=f(x)$, $d=g(y)$ independently and similarity, enabling ANN over millions but compressing each document into one vector. A cross-encoder concatenates/attends query and candidate jointly and outputs a relevance score, capturing exact interactions but requiring one forward per pair and no precomputed scalar. ColBERT-style late interaction precomputes document token vectors and scores $\sum_i\max_j q_i^Td_j$, retaining fine matching with larger indexes. LLM reranking may pointwise score, pairwise compare, or listwise reorder using instructions/evidence; it can reason about constraints but has position/verbosity bias, token cost, and harder calibration.

```text
corpus -> bi-encoder/lexical top 1000 -> late/cross rerank top 100 -> LLM/rules top 10 -> context
         high recall, cheap             precision                selective expensive
```

Train rerankers with judged/hard negatives and evaluate nDCG/Recall after each stage. Preserve IDs and filter authorization before candidate content reaches models.
* **Production Reality & Tradeoffs:** Reranking cannot recover missing candidates. Long documents need windows/aggregation. Batch cross-encoder pairs; cap LLM use or cascade by ambiguity. Monitor stage latency and recall loss.
* **Red Flag:** Applying an expensive reranker to the whole corpus or reporting reranker quality without first-stage recall.

