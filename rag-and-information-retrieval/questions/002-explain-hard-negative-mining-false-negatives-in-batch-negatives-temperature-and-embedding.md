### Q: Explain hard-negative mining, false negatives, in-batch negatives, temperature, and embedding collapse.
* **Difficulty:** Senior
* **Category:** Deep Learning
* **The 10-Second Pitch:** Contrastive retrievers learn relative scores against sampled negatives. Hard negatives supply useful gradient near the boundary; false negatives repel relevant documents; temperature controls concentration; collapse is diagnosed by geometry and loss, not prevented by negative count alone.
* **The Deep Dive:** For query $q$, positive $d^+$, and candidate set $C$,

$$
L=-\log\frac{e^{s(q,d^+)/\tau}}{\sum_{d\in C}e^{s(q,d)/\tau}}.
$$

In-batch negatives reuse other examples’ documents, giving a $B\times B$ matrix cheaply, but duplicates/multiple relevant documents become false negatives. Random negatives soon score too low and contribute negligible gradient; hard negatives from BM25/ANN/teacher/current checkpoint teach fine distinctions. Extremely hard negatives may be mislabeled or impossible. Mine with a stale/frozen checkpoint, filter known positives/near duplicates, mix hardness levels, refresh periodically, and avoid using test judgments.

Lower $\tau$ sharpens probabilities and emphasizes highest-scoring cases, amplifying label noise. Collapse appears as low embedding variance/rank, near-identical cosine scores, uniform retrieval, or pathological norm growth; normalization and negatives help but data/objective/optimization remain causal. Cross-device all-gather must keep correct labels and gradient semantics.
* **Production Reality & Tradeoffs:** Mining is an expensive feedback loop and can narrow the model to retriever-specific artifacts. Evaluate recall, hard slices, calibration, and cross-corpus shift. Monitor norm, covariance spectrum, positive/negative score distributions, and false-negative audits.
* **Red Flag:** Calling every nonclicked document irrelevant, or mining top results and treating them as negatives without relevance review.

