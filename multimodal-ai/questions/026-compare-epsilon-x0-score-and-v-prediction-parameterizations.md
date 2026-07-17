### Q: Compare epsilon, x0, score, and v prediction parameterizations.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Equivalent targets encode different signal-to-noise weighting: epsilon predicts added noise, x0 predicts clean data, score predicts gradient of log noisy density, and v combines noise/data to balance across timesteps.
* **The Deep Dive:** Given $x_t=\alpha_t x_0+\sigma_t\epsilon$, the epsilon target is $\epsilon$, the clean-sample target is $x_0$, and the score is $\nabla_{x_t}\log p_t(x_t)\approx-\epsilon/\sigma_t$. Under a common normalized schedule, velocity is $v=\alpha_t\epsilon-\sigma_t x_0$. These targets are algebraically convertible when $\alpha_t$ and $\sigma_t$ are known, but their MSE losses weight signal-to-noise regions differently and network approximation errors do not transform exactly. Velocity prediction often balances high- and low-noise behavior better.
* **Production Reality & Tradeoffs:** Schedulers and libraries use different coefficient conventions. Loss weighting, latent scaling, clipping, and precision affect results. Do not convert checkpoints without matching training parameterization.
The parameterizations are algebraically convertible given the noise schedule. With $x_t=\alpha_t x_0+\sigma_t\epsilon$, predicting $\epsilon$ gives $\hat x_0=(x_t-\sigma_t\hat\epsilon)/\alpha_t$; the score is $\nabla_{x_t}\log p_t(x_t)\approx-\hat\epsilon/\sigma_t$; velocity commonly uses $v=\alpha_t\epsilon-\sigma_t x_0$. Their target scales and signal-to-noise weighting differ across timesteps, affecting numerical stability. State the exact schedule convention before comparing losses.

* **Red Flag:** Calling targets identical because formulas convert exactly in the noise-free limit.
