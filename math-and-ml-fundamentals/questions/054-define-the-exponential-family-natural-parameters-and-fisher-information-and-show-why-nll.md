### Q: Define the exponential family, natural parameters, and Fisher information, and show why NLL gradients take the prediction-minus-target form across GLMs.
* **Difficulty:** Senior
* **Category:** Probabilistic Modeling
* **The 10-Second Pitch:** An exponential-family density is $h(y)\exp(\eta^{\top}T(y)-A(\eta))$ with natural parameter $\eta$; the log-partition $A$ satisfies $\nabla A=\mathbb{E}[T(y)]$ and $\nabla^2 A=\operatorname{Cov}[T(y)]$, so the NLL gradient in $\eta$ is exactly $\hat y-y$. Every canonical-link GLM — linear, logistic, softmax, Poisson — inherits residual-times-input gradients from this one identity, and Fisher information is just $\nabla^2 A$, making natural gradient equal Newton's method there.
* **The Deep Dive:** The family is $p(y\mid\eta)=h(y)\exp\!\big(\eta^{\top}T(y)-A(\eta)\big)$ with sufficient statistic $T(y)$ and log-partition $A(\eta)=\log\int h(y)e^{\eta^{\top}T(y)}\,dy$, which exists precisely to normalize the density. Differentiating $A$ under the integral gives the two identities everything else rests on:

  $$
  \nabla A(\eta)=\int T(y)\,\frac{h(y)e^{\eta^{\top}T(y)}}{e^{A(\eta)}}\,dy=\mathbb{E}_{\eta}[T(y)],
  \qquad
  \nabla^2 A(\eta)=\operatorname{Cov}_{\eta}[T(y)]\succeq 0,
  $$

  so $A$ is convex and the NLL is convex in $\eta$. Concrete instances: Bernoulli has $T(y)=y$, $\eta=\log\frac{p}{1-p}$, $A(\eta)=\log(1+e^{\eta})$; fixed-variance Gaussian has $\eta=\mu$, $A(\eta)=\eta^2/2$; Poisson has $\eta=\log\lambda$, $A(\eta)=e^{\eta}$; the categorical gives softmax.

  The prediction-minus-target form is now a two-line derivation. Since $-\log p(y\mid\eta)=A(\eta)-\eta^{\top}T(y)-\log h(y)$,

  $$
  \nabla_{\eta}\big(-\log p(y\mid\eta)\big)=\nabla A(\eta)-T(y)=\hat y-y
  \quad\text{for } T(y)=y,\ \hat y:=\mathbb{E}_{\eta}[y].
  $$

  A GLM with the canonical link sets $\eta=w^{\top}x$, so the chain rule gives $\nabla_w=(\hat y-y)\,x$ with no link-derivative factor — it cancels identically. Check the instances: logistic regression gives $(\sigma(w^{\top}x)-y)\,x$; linear regression gives the residual times $x$; softmax cross-entropy gives $(p-\text{onehot})$; Poisson log-link gives $(e^{w^{\top}x}-y)\,x$. Worked example: logistic with $w=0$, $x=(1,2)$, $y=1$ predicts $\hat y=0.5$, so the gradient is $(0.5-1)\cdot(1,2)=(-0.5,-1)$.

  Fisher information is defined as $F(\eta)=\mathbb{E}\big[\nabla_{\eta}\log p\,(\nabla_{\eta}\log p)^{\top}\big]=-\mathbb{E}\big[\nabla_{\eta}^2\log p\big]$. In the exponential family $\nabla_{\eta}^2\log p=-\nabla^2 A(\eta)$ contains no $y$, so the expectation is trivial and $F(\eta)=\nabla^2 A(\eta)=\operatorname{Cov}[T(y)]$: for Bernoulli, $F(\eta)=\sigma(\eta)(1-\sigma(\eta))$, equal to $0.25$ at $\eta=0$. The natural gradient $F^{-1}\nabla$ is the steepest-descent direction measured in KL rather than Euclidean distance and is invariant to smooth reparameterization; because Fisher equals the exact Hessian here, natural gradient on a canonical-link GLM is Newton's method — iteratively reweighted least squares with weights $\hat y(1-\hat y)$. Continuing the example with scalar $x=1$: gradient $-0.5$, Fisher $x^2\cdot 0.25=0.25$, natural-gradient step $F^{-1}\nabla=-2$.

  The limiting case that falsifies overgeneralization is a non-canonical link. Probit regression models $\hat y=\Phi(\eta)$ with $\eta=w^{\top}x$: the gradient carries an extra factor $\phi(\eta)/\big(\Phi(\eta)(1-\Phi(\eta))\big)$ that no longer cancels. Numerically, at $\eta=2$, $y=0$: the derivative of the NLL is $\phi(2)/(1-\Phi(2))\approx 0.054/0.0228\approx 2.37$, while $\hat y-y\approx 0.977$ — off by a factor of about $2.4$. A finite-difference check on the probit NLL exposes this immediately, so "gradient equals residual" is a theorem about canonical links, not about GLMs in general.
* **Production Reality & Tradeoffs:** The $\hat y-y$ form is why frameworks fuse softmax with cross-entropy — the combined backward pass is the numerically benign $p-\text{onehot}$ instead of dividing by tiny probabilities — and why last-layer gradients stay well scaled, with label smoothing acting as a target shift inside the same formula. Exact natural gradient is quadratic-cost in parameters, so at scale it survives only as approximations (K-FAC's Kronecker factors, Adam's diagonal rescaling); beware the empirical Fisher, which plugs in observed labels instead of model samples and can be badly conditioned as an approximation to $F$.
* **Red Flag:** Asserting that any loss yields prediction-minus-target gradients — the probit factor above disproves it — or conflating empirical Fisher with true Fisher; the identity $F=\nabla^2 A$ needs the expectation under the model, not under the data labels.
