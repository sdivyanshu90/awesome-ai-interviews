### Q: Derive gradients for sigmoid, tanh, ReLU, GELU, softmax, log-softmax, and normalization.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Elementwise activation derivatives are local; softmax and normalization couple coordinates. Efficient autodiff uses vector-Jacobian products such as $p\odot(g-\langle g,p\rangle)$ rather than materializing dense Jacobians.
* **The Deep Dive:** $\sigma'(x)=\sigma(x)(1-\sigma(x))$; $\tanh'(x)=1-\tanh^2x$; ReLU derivative is $1_{x>0}$ away from zero. Exact GELU $x\Phi(x)$ has derivative $\Phi(x)+x\phi(x)$. For $p_i=e^{z_i}/\sum_ke^{z_k}$,

$$
\frac{\partial p_i}{\partial z_j}=p_i(\delta_{ij}-p_j),\qquad
J=\operatorname{diag}(p)-pp^T.
$$

Given upstream $g$, the VJP is $J^Tg=p\odot(g-\langle g,p\rangle)$. Log-softmax $\ell_i=z_i-\operatorname{LSE}(z)$ has $\partial\ell_i/\partial z_j=\delta_{ij}-p_j$; fused CE gives $p-y$.

For LayerNorm over $d$ features, $\hat x=(x-\mu)r$, $r=(\sigma^2+\epsilon)^{-1/2}$. If upstream after scale is $g$, then

$$
\frac{\partial L}{\partial x}=\frac{r}{d}\left(dg-\sum_i g_i-\hat x\sum_i g_i\hat x_i\right).
$$

This shows every feature affects mean/variance and therefore every other gradient.
* **Production Reality & Tradeoffs:** Use fused stable kernels and correct reduction axes. Saturated sigmoid/tanh gradients vanish; approximate GELU changes derivative slightly. Fully masked softmax rows require defined behavior. Normalization epsilon/precision materially changes small-variance cases.
* **Red Flag:** Treating softmax or LayerNorm gradients as independent elementwise derivatives.

