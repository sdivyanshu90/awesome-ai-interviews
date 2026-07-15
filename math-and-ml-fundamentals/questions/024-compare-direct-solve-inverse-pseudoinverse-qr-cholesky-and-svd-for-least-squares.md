### Q: Compare direct solve, inverse, pseudoinverse, QR, Cholesky, and SVD for least squares.
* **Difficulty:** Senior
* **Category:** Numerical Methods
* **The 10-Second Pitch:** Solve systems through factorizations, not explicit inverses. LU/direct solve handles square systems, Cholesky handles SPD matrices, QR is stable for full-rank least squares, and SVD/pseudoinverse handles rank deficiency at higher cost.
* **The Deep Dive:** For square nonsingular $Ax=b$, factorization plus triangular solves is cheaper and more stable than forming $A^{-1}$. Least squares minimizes $\|Ax-b\|_2$; normal equations $A^TAx=A^Tb$ square the condition number. With thin QR, $A=QR$, solve $Rx=Q^Tb$ without that squaring. If a symmetric positive-definite matrix $H$ is already given, Cholesky $H=LL^T$ is efficient; using it on $A^TA$ retains normal-equation conditioning.

SVD $A=U\Sigma V^T$ yields the Moore–Penrose solution

$$
x=A^+b=V\Sigma^+U^Tb,
$$

which is the minimum-norm least-squares solution. Thresholding small $\sigma_i$ determines numerical rank; Tikhonov regularization replaces $1/\sigma_i$ by $\sigma_i/(\sigma_i^2+\lambda)$. For many right-hand sides, factor once and solve repeatedly. Iterative CG/LSQR is appropriate when matrices are huge/sparse and only matrix-vector products exist.
* **Production Reality & Tradeoffs:** Pivoting matters for LU/QR; rank thresholds depend on scale/precision. SVD is robust but expensive. Never explicitly build sparse $A^TA$ if it destroys sparsity or conditioning. Measure residual and backward error, not just whether the solver returned.
* **Red Flag:** Using `inv(A) @ b`, or recommending Cholesky merely because a matrix is square.

