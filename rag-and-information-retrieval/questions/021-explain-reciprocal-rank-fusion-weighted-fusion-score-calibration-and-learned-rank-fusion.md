### Q: Explain reciprocal-rank fusion, weighted fusion, score calibration, and learned rank fusion.
* **Difficulty:** Senior
* **Category:** Retrieval
* **The 10-Second Pitch:** Fusion combines complementary retrievers. RRF uses only ranks and is robust to incomparable scores; weighted fusion combines normalized/calibrated scores; learned fusion predicts relevance from multiple signals but needs unbiased labels.
* **The Deep Dive:** Reciprocal-rank fusion assigns

$$
\operatorname{RRF}(d)=\sum_{r\in\mathcal R}\frac{1}{k+\operatorname{rank}_r(d)},
$$

where an absent document contributes zero and $k$ dampens top-rank dominance. Weighted fusion requires comparable scales—per-query min-max scaling is unstable, whereas fitted calibration can map each retriever score to relevance probability. Features may include lexical/vector ranks, exact entity matches, freshness, authority, and click signals. Learning-to-rank objectives optimize pairwise or listwise order but inherit position and selection bias from logs.

Ranks are one-based, so the top document contributes $1/(k+1)$; a common default is $k=60$. Because RRF discards score magnitude entirely, it is robust to incomparable scales but surrenders the calibrated confidence that downstream thresholds or abstention logic may need. Deduplicate to canonical document/span identities before or during fusion so duplicated chunks cannot gain multiple votes.
* **Production Reality & Tradeoffs:** Fuse only authorized candidates. Missing candidates, duplicate chunks, and correlated retrievers affect gains. Measure per-query-class improvement and latency; a learned ranker needs monitoring and rollback.
* **Red Flag:** Adding raw BM25 and cosine scores without calibration.
