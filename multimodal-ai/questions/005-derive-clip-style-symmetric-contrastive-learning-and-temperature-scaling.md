### Q: Derive CLIP-style symmetric contrastive learning and temperature scaling.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** CLIP maps image and text to normalized vectors and applies cross-entropy in both image-to-text and text-to-image directions over an in-batch similarity matrix. A learned temperature controls logit sharpness.
* **The Deep Dive:** For a global batch of $B$ aligned pairs, encoders produce $a_i=f(x_i)$ and $b_i=g(y_i)$, then normalize $u_i=a_i/\lVert a_i\rVert_2$ and $v_i=b_i/\lVert b_i\rVert_2$. With temperature $\tau>0$,

$$
S_{ij}=\frac{u_i^\top v_j}{\tau},\qquad
\mathcal L_{i\rightarrow t}=-\frac1B\sum_{i=1}^{B}
\log\frac{\exp(S_{ii})}{\sum_{j=1}^{B}\exp(S_{ij})},
$$

and $\mathcal L=(\mathcal L_{i\rightarrow t}+\mathcal L_{t\rightarrow i})/2$, where the reverse term applies cross-entropy to columns. Normalization prevents embedding-norm inflation from solving the task. A learned logit scale $s=\log(1/\tau)$ is usually bounded; very small $\tau$ concentrates gradients on hard examples and amplifies false-negative or annotation noise.

  Each other batch item is treated as negative. That assumption breaks for duplicate images, paraphrases, and multiple valid captions; a semantically valid pair is then repelled. Deduplication, multi-positive masks, or sigmoid pairwise losses change this geometry. Distributed training all-gathers embeddings before forming global logits, but gradients and labels must preserve rank offsets. A fully collapsed representation makes every probability $1/B$ and loss $\log B$; it is not optimal if positives are learnable, although weak data or optimization can still yield poor feature variance.
* **Production Reality & Tradeoffs:** Large global batches improve negative coverage but add $O(B²)$ similarity work and collective traffic. Caption quality, geographic/language coverage, and shortcut correlations frequently dominate encoder scale. Evaluate both retrieval directions, zero-shot classification across prompt templates, calibration, subgroup slices, and downstream grounding: high retrieval recall does not establish counting, localization, OCR, or generation competence.
* **Red Flag:** Describing CLIP as a caption generator.
