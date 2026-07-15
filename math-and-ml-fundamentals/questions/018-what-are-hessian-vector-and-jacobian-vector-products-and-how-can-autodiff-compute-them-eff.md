### Q: What are Hessian-vector and Jacobian-vector products, and how can autodiff compute them efficiently?
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** A JVP computes $Jv$ by forward-mode directional differentiation; a VJP computes $u^TJ$ by reverse mode; an HVP computes $Hv$ without materializing the $n\times n$ Hessian, enabling curvature diagnostics and implicit methods.
* **The Deep Dive:** For $f:\mathbb R^n\to\mathbb R^m$, $J_{ij}=\partial f_i/\partial x_j$. A JVP is the directional derivative

$$
Jv=\left.\frac{d}{d\epsilon}f(x+\epsilon v)\right|_{\epsilon=0}.
$$

A VJP $u^TJ$ is what backprop computes from upstream covector $u$. For scalar loss $L$, $H=\nabla^2L$ and Pearlmutter’s identity gives

$$
Hv=\nabla_x\left(\nabla_xL(x)^Tv\right),
$$

computed as reverse-over-forward or forward-over-reverse AD at roughly gradient-scale cost and $O(n)$ storage, not $O(n^2)$. Conjugate gradients uses HVPs to solve damped systems; Lanczos estimates extreme eigenvalues; influence/implicit differentiation uses inverse-Hessian-vector products without forming the inverse.

At nonsmooth points, the returned derivative follows framework conventions and a classical Hessian may not exist. Gauss–Newton/Fisher approximations can be positive semidefinite when the true Hessian is indefinite.
* **Production Reality & Tradeoffs:** Repeated HVPs are still expensive at LLM scale and sensitive to stochastic batches, damping, and mixed precision. Curvature from one minibatch may not represent the population. Validate HVP implementations with finite-difference gradients on small smooth problems.
* **Red Flag:** Proposing to materialize the full Hessian for billions of parameters, or confusing $Jv$ with $v^TJ$.

