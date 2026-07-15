### Q: Derive Bayes’ rule and distinguish prior, likelihood, evidence, posterior, and predictive distribution.
* **Difficulty:** Mid
* **Category:** Probability
* **The 10-Second Pitch:** Bayes follows from two factorizations of the joint: posterior equals likelihood times prior divided by evidence. Prediction integrates the likelihood for a new observation over posterior parameter uncertainty.
* **The Deep Dive:** Since $p(\theta,D)=p(D\mid\theta)p(\theta)=p(\theta\mid D)p(D)$,

$$
p(\theta\mid D)=\frac{p(D\mid\theta)p(\theta)}{p(D)},\qquad
p(D)=\int p(D\mid\theta)p(\theta)d\theta.
$$

The prior is a distribution over $\theta$ before these data; the likelihood is a function of $\theta$ for fixed observed $D$ and need not integrate to one over $\theta$; evidence/marginal likelihood normalizes and compares models while automatically reflecting prior volume; posterior is updated uncertainty. Posterior predictive is

$$
p(y_*\mid x_*,D)=\int p(y_*\mid x_*,\theta)p(\theta\mid D)d\theta.
$$

For a Beta$(\alpha,\beta)$ prior and Bernoulli observations with $s$ successes and $f$ failures, the posterior is Beta$(\alpha+s,\beta+f)$ and next-success probability is $(\alpha+s)/(\alpha+\beta+s+f)$. Odds form: posterior odds = prior odds × Bayes factor.
* **Production Reality & Tradeoffs:** Priors matter when data are weak and can dominate evidence comparisons. High-dimensional evidence/posterior integrals require approximation. Dataset shift violates the assumed likelihood; a mathematically coherent posterior can still be operationally wrong.
* **Red Flag:** Calling $p(D\mid\theta)$ the probability that parameters are true, or predicting only with the MAP estimate while claiming full Bayesian uncertainty.

