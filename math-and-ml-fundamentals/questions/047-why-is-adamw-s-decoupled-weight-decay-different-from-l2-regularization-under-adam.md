### Q: Why is AdamW’s decoupled weight decay different from L2 regularization under Adam?
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** Adding $\lambda\theta$ to Adam’s gradient sends the penalty through coordinate-wise moments; AdamW applies direct shrinkage outside the preconditioner, so its decay has consistent parameter-space semantics tied to the learning rate.
* **The Deep Dive:** Adam forms $m_t,v_t$ from input gradient and updates approximately $-\eta m_t/(\sqrt{v_t}+\epsilon)$. With L2 regularization, use $g'_t=g_t+\lambda\theta_t$; the penalty changes both moments, so a coordinate’s shrinkage depends on its gradient history and preconditioner. Decoupled AdamW instead computes moments from $g_t$ and applies

$$
\theta_{t+1}=(1-\eta_t\lambda)\theta_t-\eta_t\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}.
$$

For plain SGD without momentum/preconditioning, L2 and weight decay can be equivalent after matching coefficients; adaptive scaling breaks that equivalence. The integrated shrink over a schedule is approximately $\exp(-\lambda\sum_t\eta_t)$ for small steps, so schedule and decay cannot be tuned independently in effect. Parameter groups commonly exclude biases and normalization scales because their magnitude has different semantics; this is empirical policy, not a theorem.
* **Production Reality & Tradeoffs:** Framework APIs differ on coefficient and order, and fused optimizers may implement details differently. Excess decay under long schedules erases weights; zero decay can worsen generalization. Log update/weight norms per group and reproduce exclusions.
* **Red Flag:** Saying AdamW is Adam plus an L2 term, or using the same decay unchanged after radically changing schedule length.

