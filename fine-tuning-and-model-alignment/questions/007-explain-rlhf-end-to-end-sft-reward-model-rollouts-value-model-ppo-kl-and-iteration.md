### Q: Explain RLHF end to end: SFT, reward model, rollouts, value model, PPO, KL, and iteration.
* **Difficulty:** Principal
* **Category:** Alignment
* **The 10-Second Pitch:** RLHF commonly trains an SFT policy, learns a reward model from preferences, then optimizes the policy for reward while penalizing divergence from a reference. PPO limits destructive policy steps by clipping the likelihood ratio in its surrogate objective.
* **The Deep Dive:** For advantage $A_t$ and ratio $r_t=\pi_\theta(a_t|s_t)/\pi_{old}(a_t|s_t)$, PPO maximizes $\min(r_tA_t,\operatorname{clip}(r_t,1-\epsilon,1+\epsilon)A_t)$. A KL term to the reference preserves language quality and constrains reward exploitation. Online samples, reward inference, distributed rollouts, and value estimation make this much more operationally complex than SFT.

  ```text
  demonstrations ──SFT──> policy πref/πold
                              │ generate responses
  preference pairs ──> reward model rφ(x,y)
                              │ score rollouts
                              ▼
  policy πθ + value Vψ ──PPO clipped update + KL(πθ || πref)
                              │
                              └──> new rollouts → evaluation → next iteration
  ```
Token advantages commonly use
$$
\hat A_t=\sum_{l\ge0}(\gamma\lambda)^l\delta_{t+l},\quad
\delta_t=r_t+\gamma V(s_{t+1})-V(s_t).
$$
Freeze the rollout snapshot during PPO epochs and align policy/reference masks. Monitor clip fraction, KL, entropy, value error, reward components, length, and independent human utility. Rising reward with collapsing entropy or growing KL is a failure trace.

* **Production Reality & Tradeoffs:** Tune reward scale, KL coefficient, rollout diversity, and stop criteria against a broad suite. PPO can be expensive and unstable; do not use it when high-quality supervised or direct-preference methods suffice.
* **Red Flag:** “PPO maximizes the reward model” without KL control, policy ratio, or reward-hacking discussion.
