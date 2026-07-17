### Q: Derive DPO from KL-regularized reward maximization and reference-relative log odds.
* **Difficulty:** Principal
* **Category:** Math
* **The 10-Second Pitch:** DPO optimizes preferred-versus-rejected log-probability differences relative to a reference policy, eliminating an explicit reward-model and online RL loop. It is simpler and stable offline, but depends on preference-data coverage and cannot explore new policy behavior as directly as online RL.
* **The Deep Dive:** The DPO loss is a logistic objective over $\beta[\log\pi_\theta(y_w|x)-\log\pi_{ref}(y_w|x)-\log\pi_\theta(y_l|x)+\log\pi_{ref}(y_l|x)]$. It follows from a KL-regularized reward-maximization formulation. $\beta$ controls how strongly preference differences move the model away from reference. Token masking and length effects must be handled consistently.
Let $\Delta_\theta=\log\pi_\theta(y_w|x)-\log\pi_\theta(y_l|x)$ and define $\Delta_{ref}$ similarly. DPO minimizes $-\log\sigma(\beta(\Delta_\theta-\Delta_{ref}))$. Use identical templates and response masks; length normalization changes the estimator. The reference anchors the implicit reward. DPO removes online rollout/value infrastructure but cannot explore beyond collected candidates, so measure win rate with KL, length, calibration, and retention.

* **Production Reality & Tradeoffs:** DPO is attractive for reproducible offline pipelines, but pair quality, response length bias, and out-of-distribution prompts remain critical. Compare against SFT and rejection sampling; do not assume preference optimization always improves helpfulness.
* **Red Flag:** Saying DPO is SFT on only the chosen response; the rejected response and reference-relative term are essential.
