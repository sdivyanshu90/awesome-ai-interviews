### Q: Derive PCA from constrained variance maximization and implement it using SVD.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** After centering $X\in\mathbb{R}^{n\times d}$, PCA chooses orthonormal directions maximizing projected variance. The stationary directions are eigenvectors of $X^TX/(n-1)$; computing the thin SVD $X=U\Sigma V^T$ returns them as columns of $V$ without forming the covariance matrix.
* **The Deep Dive:** For the first component, solve

$$
\max_{\|v\|_2=1}\;\frac{1}{n-1}\|Xv\|_2^2
=\max_{\|v\|_2=1}v^T\left(\frac{X^TX}{n-1}\right)v.
$$

The Lagrangian derivative gives $Cv=\lambda v$, where $C=X^TX/(n-1)$. Rayleigh–Ritz selects the largest eigenvalue; subsequent components add orthogonality constraints and select the next eigenvectors. If $X=U\Sigma V^T$, then $C=V\Sigma^2V^T/(n-1)$. The top-$k$ loading matrix is $V_k$, scores are $Z=XV_k=U_k\Sigma_k$, and explained-variance ratio is $\sigma_i^2/\sum_j\sigma_j^2$.

```text
raw X -> subtract training mean -> thin/randomized SVD -> choose k
                                             |            |
                                      V_k loadings    explained variance
                                             |
                                      Z = X_centered V_k
```

Reconstruction is $\hat X=ZV_k^T+\mu$. The squared Frobenius reconstruction error equals $\sum_{i>k}\sigma_i^2$, linking PCA to Eckart–Young. Standardization changes the question from covariance PCA to correlation PCA and can radically reorder components.
* **Production Reality & Tradeoffs:** Fit centering/scaling and components on training data only. Use randomized/incremental SVD for large matrices and sparse-compatible centering strategies. PCA is linear, sensitive to outliers, sign-indeterminate, and variance is not necessarily task relevance. Monitor drift in mean, spectrum, and subspace angles.
* **Red Flag:** Running PCA on uncentered data without explanation, forming a huge dense covariance matrix, or interpreting component sign as intrinsically meaningful.

