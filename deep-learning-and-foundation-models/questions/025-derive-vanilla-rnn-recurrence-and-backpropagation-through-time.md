### Q: Derive vanilla RNN recurrence and backpropagation through time.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** An RNN reuses one transition $h_t=\phi(W_hh_{t-1}+W_xx_t+b)$. BPTT unrolls time, recursively accumulates hidden-state adjoints, and sums shared-parameter gradients across steps; repeated Jacobian products cause vanishing/explosion.
* **The Deep Dive:** Let $a_t=W_hh_{t-1}+W_xx_t+b$, $h_t=\phi(a_t)$, and $L=\sum_t\ell_t(h_t)$. Define $\bar h_t=\partial L/\partial h_t$. Reverse recurrence is

$$
\bar h_t=\frac{\partial\ell_t}{\partial h_t}+W_h^T\left(\bar h_{t+1}\odot\phi'(a_{t+1})\right).
$$

Let $\delta_t=\bar h_t\odot\phi'(a_t)$. Since weights are shared,

$$
\frac{\partial L}{\partial W_h}=\sum_t\delta_th_{t-1}^T,\quad
\frac{\partial L}{\partial W_x}=\sum_t\delta_tx_t^T,\quad
\frac{\partial L}{\partial b}=\sum_t\delta_t.
$$

Long-range contribution contains products $\prod W_h^T\operatorname{diag}(\phi')$, controlled by input-dependent singular values. Truncated BPTT detaches state every window: forward memory may continue, but gradients beyond the boundary are biased to zero. Padding needs masks so hidden/loss updates stop appropriately.
* **Production Reality & Tradeoffs:** Full BPTT costs $O(T)$ activations and is sequential in time. Gradient clipping limits explosion but not vanishing. Stateful streaming must define reset/detach and batch reordering. Orthogonal init/gates help but do not guarantee long memory.
* **Red Flag:** Using a different $W_h$ per timestep or forgetting to sum gradients from every reuse.

