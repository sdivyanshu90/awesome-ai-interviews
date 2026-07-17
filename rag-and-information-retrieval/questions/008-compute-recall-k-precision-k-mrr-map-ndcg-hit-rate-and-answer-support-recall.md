### Q: Compute Recall@k, Precision@k, MRR, MAP, nDCG, hit rate, and answer-support recall.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Retrieval metrics answer different user needs: coverage of all relevant evidence, precision in the context budget, first-hit rank, ranking over multiple items, graded utility, any-hit success, and whether the complete answer’s support set is present.
* **The Deep Dive:** For relevance set $R_q$ and top-$k$ list, $Recall@k=|top_k\cap R_q|/|R_q|$, $Precision@k=|top_k\cap R_q|/k$, and hit rate is $1[top_k\cap R_q\ne\emptyset]$ averaged. Reciprocal rank is $1/r_1$ for first relevant; MRR averages. AP averages precision at each relevant hit and normalizes by relevant count; MAP averages queries. With graded relevance,

$$
DCG@k=\sum_{i=1}^k\frac{2^{rel_i}-1}{\log_2(i+1)},\qquad nDCG=DCG/IDCG.
$$

Answer-support recall labels atomic claims/hops and asks whether every required supporting fact/span/document is present; one relevant chunk is insufficient for multi-hop. Define relevance granularity (chunk/document/span), duplicate handling, zero-relevant queries, and incomplete judgments. Evaluate filters/freshness and oracle-generation: generation with gold context separates retrieval from answer use.
* **Production Reality & Tradeoffs:** Chunk overlap inflates hits; unjudged is not always irrelevant. Offline labels drift with corpus versions and may omit alternate support. Report per-intent/domain/time slices, uncertainty, latency, and tokens. Downstream answer delta complements—not replaces—retrieval metrics.
* **Red Flag:** Using hit rate as “retrieval accuracy” for multi-evidence questions or evaluating only final answers.

