### Q: How do low-rank approximation and the Eckart–Young theorem motivate adapters and compression?
* **Difficulty:** Principal
* **Category:** Math
* **The 10-Second Pitch:** Eckart–Young proves the truncated SVD is the best rank-$r$ approximation of an existing matrix under spectral/Frobenius norms. LoRA instead constrains the learned update $\Delta W=BA$ to rank at most $r$; the theorem motivates low-dimensional structure but does not prove task updates are low rank.
* **The Deep Dive:** If $W=U\Sigma V^T$ with $\sigma_1\ge\cdots$, then

$$
W_r=U_r\Sigma_rV_r^T,
\quad
\|W-W_r\|_2=\sigma_{r+1},
\quad
\|W-W_r\|_F^2=\sum_{i>r}\sigma_i^2.
$$

Compression applies this idea to approximate a trained weight/activation matrix, often followed by fine-tuning. LoRA freezes $W\in\mathbb R^{d_{out}\times d_{in}}$ and trains $B\in\mathbb R^{d_{out}\times r}$, $A\in\mathbb R^{r\times d_{in}}$, so $\operatorname{rank}(BA)\le r$ and trainable parameters fall from $d_{out}d_{in}$ to $r(d_{out}+d_{in})$. It does not compute the top singular vectors of $W$; it learns a task update in a constrained factorization. Gauge freedom $BA=(BR)(R^{-1}A)$ makes factors nonidentifiable. Effective numerical rank may be lower than configured rank.

Choose rank by quality/retention and inspect singular values of learned updates across modules. Layerwise ranks can outperform a uniform budget.
* **Production Reality & Tradeoffs:** Low Frobenius error does not ensure downstream quality: small singular directions may encode rare capabilities. Factorized execution saves parameters but may add kernel launches unless merged or batched with adapter-aware kernels. Merging changes rollback/composition semantics and precision.
* **Red Flag:** Claiming Eckart–Young proves LoRA always preserves model quality, or confusing compression of $W$ with low-rank learning of $\Delta W$.

