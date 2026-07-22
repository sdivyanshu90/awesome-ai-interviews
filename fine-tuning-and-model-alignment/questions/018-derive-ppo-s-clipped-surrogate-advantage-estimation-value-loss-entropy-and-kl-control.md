### Q: Derive PPO’s clipped surrogate, advantage estimation, value loss, entropy, and KL control.
* **Difficulty:** Principal
* **Category:** Math
* **The 10-Second Pitch:** PPO maximizes a clipped policy-ratio objective weighted by advantage, trains a value baseline, may add entropy, and constrains divergence from a reference/old policy. Clipping limits incentives for large updates but is not a hard trust region.
* **The Deep Dive:** Define the probability ratio against the frozen rollout policy as

$$
r_t(\theta)=\frac{\pi_\theta(a_t\mid s_t)}{\pi_{\mathrm{old}}(a_t\mid s_t)}.
$$

PPO maximizes

$$
\mathcal L_{\mathrm{clip}}(\theta)=
\mathbb E_t\!\left[
\min\!\left(r_t\hat A_t,
\operatorname{clip}(r_t,1-\epsilon,1+\epsilon)\hat A_t\right)
\right].
$$

GAE uses $\delta_t=r_t^{\mathrm{env}}+\gamma V(s_{t+1})-V(s_t)$ and $\hat A_t=\sum_{l\ge0}(\gamma\lambda)^l\delta_{t+l}$. The superscript distinguishes environment reward from the policy ratio. The full loss adds value regression, optional entropy, and often $-\beta D_{\mathrm{KL}}(\pi_\theta\|\pi_{\mathrm{ref}})$. Normalize advantages only over valid response tokens, mask padding, and monitor ratio, clip fraction, KL, entropy, value error, and each reward component.
Clipping is asymmetric through the sign of $A_t$: for positive advantage it caps excessive probability increases; for negative advantage it caps excessive decreases. It does not impose a hard sequence-level KL constraint. Verify this with scalar ratios around $1\pm\epsilon$, then check masks, stale policy log probabilities, whitening, and reward scaling in code. These details dominate PPO stability.

* **Production Reality & Tradeoffs:** Sequence rewards create credit-assignment noise; PPO rollout infrastructure is expensive. Adaptive KL and early stopping help, but reward hacking remains. Off-policy/stale rollouts break assumptions.
* **Red Flag:** Explaining clipping as guaranteeing the policy cannot move far.
