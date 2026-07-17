### Q: Derive the diffusion forward process, closed-form noising, reverse process, and ELBO connection.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** DDPM adds Gaussian noise according to a fixed variance schedule $\beta_t$; a closed form samples any $x_t$ directly from $x_0$. A learned reverse Gaussian approximates denoising transitions, and the variational objective commonly becomes a weighted noise-prediction MSE.
* **The Deep Dive:** Choose $\beta_t\in(0,1)$, define $\alpha_t=1-\beta_t$, and $\bar\alpha_t=\prod_{s=1}^{t}\alpha_s$. The forward Markov process is

$$
q(x_t\mid x_{t-1})=\mathcal N\!\left(\sqrt{\alpha_t}x_{t-1},\beta_t I\right).
$$

Composing Gaussian transitions gives

$$
q(x_t\mid x_0)=\mathcal N\!\left(\sqrt{\bar\alpha_t}x_0,(1-\bar\alpha_t)I\right),
\qquad
x_t=\sqrt{\bar\alpha_t}x_0+\sqrt{1-\bar\alpha_t}\,\epsilon.
$$

Thus training samples any timestep without simulating all previous steps. The exact posterior $q(x_{t-1}\mid x_t,x_0)$ is Gaussian with variance $\tilde\beta_t=\beta_t(1-\bar\alpha_{t-1})/(1-\bar\alpha_t)$. During generation $x_0$ is unknown, so $p_\theta(x_{t-1}\mid x_t)$ estimates its mean through predicted noise, clean sample, velocity, or score.

```text
training: x_0 -- one closed-form noising step at sampled t --> x_t -- network --> ε/x_0/v target
sampling: x_T -> x_(T-1) -> ... -> x_1 -> x_0
              p_θ             p_θ       p_θ
```

  Introducing $x_{1:T}$ yields an ELBO for $\log p_\theta(x_0)$. Its negative decomposes into a terminal prior KL, per-step $D_{KL}[q(x_{t-1}\mid x_t,x_0)\|p_\theta(x_{t-1}\mid x_t)]$, and a reconstruction term. With fixed variance and epsilon prediction, each Gaussian KL becomes a timestep-weighted $\mathbb E[w_t\lVert\epsilon-\epsilon_\theta(x_t,t)\rVert_2^2]$; the common simple loss changes or drops $w_t$. Noise MSE is therefore a reweighted variational/denoising-score-matching objective, not an unrelated heuristic.
* **Production Reality & Tradeoffs:** Ancestral sampling needs one large network evaluation per step. Latent diffusion reduces spatial compute; DDIM/solvers, distillation, consistency, and flow methods reduce evaluations. Schedule, SNR weighting, parameterization, VAE loss, guidance, and precision interact. Likelihood need not correlate with perceptual quality, prompt faithfulness, diversity, or memorization, so evaluate each separately.
* **Red Flag:** Describing diffusion as simply adding and removing random noise with no probabilistic transition model.
