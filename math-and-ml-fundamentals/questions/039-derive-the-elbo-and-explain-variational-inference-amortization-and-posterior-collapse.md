### Q: Derive the ELBO and explain variational inference, amortization, and posterior collapse.
* **Difficulty:** Senior
* **Category:** Probabilistic Modeling
* **The 10-Second Pitch:** Insert an approximate posterior $q_\phi(z\mid x)$ into the marginal likelihood: $\log p_\theta(x)=\mathrm{ELBO}+KL(q_\phi(z\mid x)\|p_\theta(z\mid x))$. Maximizing the bound trades reconstruction against prior KL; amortization shares inference, and a strong decoder can make the latent unused.
* **The Deep Dive:** Starting from $p(x)=\int p(x,z)dz$,

$$
\log p(x)=\log E_{q(z\mid x)}\left[\frac{p(x,z)}{q(z\mid x)}\right]
\ge E_q[\log p(x,z)-\log q(z\mid x)]
$$

by Jensen. Rearranging gives

$$
\mathcal L=E_q[\log p_\theta(x\mid z)]-KL(q_\phi(z\mid x)\|p(z)).
$$

The gap is $KL(q\|p(z\mid x))$. Amortized VI uses one encoder $\phi$ instead of optimizing variational parameters per example, introducing approximation and amortization gaps. For Gaussian $q$, $z=\mu_\phi(x)+\sigma_\phi(x)\odot\epsilon$ enables low-variance pathwise gradients.

Posterior collapse occurs when $q(z\mid x)\approx p(z)$ and decoder ignores $z$, often because an autoregressive decoder explains $x$ without latent information. Diagnose KL per dimension and mutual information/latent interventions. KL warmup, free bits, decoder weakening, skip connections, richer priors/posteriors, or altered objectives can help but change the modeling trade-off.
* **Production Reality & Tradeoffs:** ELBO values depend on likelihood parameterization and are not perceptual quality. Reverse-KL VI often undercovers modes. Importance-weighted bounds cost samples and may degrade inference-network gradients. Avoid declaring success from reconstruction alone.
* **Red Flag:** Calling the ELBO equal to log likelihood, or saying low KL always means good regularization rather than possible collapse.

