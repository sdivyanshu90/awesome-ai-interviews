### Q: How do eigenvalues, eigenvectors, singular values, and spectral radius affect neural-network dynamics?
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Eigenvalues describe invariant-direction scaling of square operators; singular values describe worst-case norm stretching of any linear map; spectral radius controls asymptotic powers under important assumptions. Neural stability is governed by products of input-dependent Jacobians, so singular spectra are usually the safer gradient diagnostic.
* **The Deep Dive:** For square $A$, $Av_i=\lambda_i v_i$ and $\rho(A)=\max_i|\lambda_i|$. If $A$ is diagonalizable, $A^t=V\Lambda^tV^{-1}$, suggesting decay for $\rho<1$ and growth for $\rho>1$. But nonnormal matrices can exhibit large transient growth even with $\rho<1$ because nonorthogonal eigenvectors make $V$ ill-conditioned.

SVD applies to any $A$: $A=U\Sigma V^T$ and

$$
\sigma_{max}(A)=\max_{\|x\|=1}\|Ax\|,
\qquad
\sigma_{min}(A)=\min_{\|x\|=1}\|Ax\|.
$$

For an RNN $h_t=\phi(Wh_{t-1})$, the time-$t$ gradient contains $\prod_k D_kW$, where $D_k=\operatorname{diag}(\phi'(a_k))$. Bounds on singular values explain vanishing/explosion, while eigenvalues of $W$ alone ignore activation derivatives and transient effects. In deep residual networks, each block Jacobian is approximately $I+J_f$, which creates an identity gradient path but does not guarantee stability. Hessian eigenvalues describe local loss curvature: a stable constant step on a positive quadratic needs $0<\eta<2/\lambda_{max}$.
* **Production Reality & Tradeoffs:** Spectral normalization constrains a layer’s Lipschitz upper bound but may reduce capacity and does not tightly bound a whole nonlinear network. Estimate spectra with power iteration/Lanczos and inspect activation/gradient norms. Weight spectra can look benign while attention, normalization, gating, or data-dependent Jacobians destabilize training.
* **Red Flag:** Using spectral radius and largest singular value interchangeably, or diagnosing a nonlinear network only from eigenvalues of one weight matrix.

