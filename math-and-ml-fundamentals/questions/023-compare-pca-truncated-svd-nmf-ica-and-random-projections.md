### Q: Compare PCA, truncated SVD, NMF, ICA, and random projections.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** PCA optimizes centered variance/reconstruction with orthogonal components; truncated SVD optimizes low-rank reconstruction without requiring centering; NMF imposes nonnegative parts; ICA targets statistical independence; random projections preserve geometry probabilistically without fitting.
* **The Deep Dive:** Centered PCA solves $\min_{\operatorname{rank}(\hat X)\le k}\|X-\hat X\|_F$ with an affine mean plus orthogonal subspace. Truncated SVD applies the Eckart–Young solution $U_k\Sigma_kV_k^T$ directly, useful for sparse term-document matrices where centering destroys sparsity. NMF solves $X\approx WH$ with $W,H\ge0$; it is nonconvex, scale/permutation nonidentifiable, and often yields interpretable additive parts. ICA assumes observed mixtures $x=As$ and estimates components that are maximally non-Gaussian/independent; whitening alone only decorrelates. A random projection $R\in\mathbb R^{d\times k}$ uses no data fit; Johnson–Lindenstrauss gives $k=O(\log n/\epsilon^2)$ to preserve pairwise distances within $1\pm\epsilon$ with high probability.

| Method | Key objective/assumption | Main strength |
|---|---|---|
| PCA | centered variance, orthogonality | denoising/compression |
| SVD | best low-rank matrix fit | sparse/latent semantics |
| NMF | nonnegative additivity | parts interpretability |
| ICA | independent non-Gaussian sources | source separation |
| RP | random geometric embedding | fast streaming/sketching |
* **Production Reality & Tradeoffs:** Select with downstream validation, not explained variance alone. PCA/NMF are scale- and outlier-sensitive; ICA assumptions often fail; NMF initialization changes solutions; random projection is not learned denoising. Fit every data-dependent transform on training data only.
* **Red Flag:** Calling all five methods interchangeable dimensionality reduction or claiming PCA components are independent because they are uncorrelated.

