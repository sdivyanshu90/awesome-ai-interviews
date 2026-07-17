### Q: Derive contrastive, triplet, InfoNCE, multiple-negative, and margin-ranking losses for retrievers.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Retriever losses compare positive scores against sampled negatives: margin losses require a fixed separation; InfoNCE uses softmax competition and temperature; multiple-negative training uses other batch documents as negatives. Sampling defines what ranking is learned.
* **The Deep Dive:** For positive $d^+$ and negative $d^-$, triplet/margin loss is

$$
L=\max(0,m+s(q,d^-)-s(q,d^+)).
$$

It has no gradient after margin satisfaction. InfoNCE over candidates $C$ is

$$
L=-\log\frac{e^{s(q,d^+)/\tau}}{\sum_{d\in C}e^{s(q,d)/\tau}}.
$$

Temperature controls hard-example concentration. In-batch multiple negatives form score matrix $S_{ij}=s(q_i,d_j)$ with diagonal positives; cross-device gathering increases candidates. Symmetric loss additionally treats document-to-query direction, useful when that geometry matters. A pairwise logistic loss $\log(1+e^{s^- - s^+})$ is a smooth margin alternative. Classical contrastive metric loss pulls positives and penalizes negative distances within a margin.

False negatives violate objectives; multi-positive masks/graded relevance are needed. Hard-negative mining changes gradient distribution and must preserve test separation. Normalize if cosine is intended.
* **Production Reality & Tradeoffs:** Large batches improve negatives but cost communication and raise false negatives. Loss value across batch sizes/temperatures is not comparable. Evaluate retrieval and score distributions; a low training loss can reflect trivial negatives.
* **Red Flag:** Discussing the loss without the negative-sampling distribution or assuming every off-diagonal pair is irrelevant.

