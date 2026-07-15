### Q: What is matrix conditioning, and why does forming $X^TX$ square the condition number?
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** The 2-norm condition number is $\kappa_2(X)=\sigma_{max}(X)/\sigma_{min}(X)$. Because the eigenvalues of $X^TX$ are $\sigma_i(X)^2$, $\kappa_2(X^TX)=\kappa_2(X)^2$, so normal equations magnify numerical error and optimization anisotropy.
* **The Deep Dive:** For a full-column-rank least-squares problem, perturbation analysis gives a relative solution error proportional to the condition number: poorly conditioned directions turn small data or rounding errors into large coefficient changes. With SVD $X=U\Sigma V^T$,

$$
X^TX=V\Sigma^2V^T,
\qquad
\lambda_i(X^TX)=\sigma_i(X)^2.
$$

Therefore

$$
\kappa_2(X^TX)=\frac{\sigma_{max}^2}{\sigma_{min}^2}=\kappa_2(X)^2.
$$

If $\kappa(X)=10^8$, forming $X^TX$ produces a condition number near $10^{16}$—roughly the reciprocal of FP64 machine epsilon—so distinguishable directions can disappear numerically. The same geometry affects gradient descent on quadratic loss $\frac12\|Xw-y\|^2$: its Hessian is $X^TX$, and convergence along eigen-direction $i$ is governed by $1-\eta\sigma_i^2$. One learning rate must be safe for the largest curvature while progress along the smallest can be extremely slow.

QR solves least squares without explicitly squaring the spectrum; SVD additionally reveals rank and supports truncated or Tikhonov-regularized solutions. Feature scaling and preconditioning change the coordinate geometry rather than adding information.
* **Production Reality & Tradeoffs:** Condition estimates should use iterative or factorization-based routines, not an explicit inverse. In deep learning, local Jacobians/Hessians are input-dependent, so a single weight-matrix condition number is only a diagnostic. Mixed precision makes the danger larger, while regularization can intentionally suppress poorly identified directions.
* **Red Flag:** Saying $X^TX$ is safer because it is symmetric, or computing `inv(X.T @ X) @ X.T @ y` for an ill-conditioned problem.

