### Q: State convexity conditions, derive the Lagrangian dual and KKT conditions, and show how KL-constrained reward maximization yields a closed-form optimal policy.
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** A convex problem minimizes a convex objective over convex inequality and affine equality constraints; the Lagrangian dual lower-bounds it always, is tight under Slater's condition, and KKT conditions are then necessary and sufficient. Applying KKT stationarity to maximizing expected reward minus $\beta$ times KL from a reference policy gives $\pi^{\star}(y\mid x)\propto\pi_{\mathrm{ref}}(y\mid x)\exp\!\big(r(x,y)/\beta\big)$ — the closed form DPO inverts.
* **The Deep Dive:** A function $f$ is convex when its domain is convex and $f(\lambda u+(1-\lambda)v)\le\lambda f(u)+(1-\lambda)f(v)$ for $\lambda\in[0,1]$; if differentiable, equivalently $f(v)\ge f(u)+\nabla f(u)^{\top}(v-u)$ (tangents underestimate), and if twice differentiable, $\nabla^2 f\succeq 0$. For the problem $\min_u f(u)$ subject to $g_i(u)\le 0$ and $h_j(u)=0$, the Lagrangian is $\mathcal{L}(u,\lambda,\nu)=f(u)+\sum_i\lambda_i g_i(u)+\sum_j\nu_j h_j(u)$ and the dual function $d(\lambda,\nu)=\inf_u\mathcal{L}$ is concave regardless of $f$; weak duality $d(\lambda,\nu)\le p^{\star}$ always holds because for feasible $u$ and $\lambda\ge 0$ the added terms are nonpositive. When $f$ and each $g_i$ are convex, the $h_j$ affine, and a strictly feasible point exists (Slater), strong duality holds and any optimum satisfies KKT: primal feasibility, dual feasibility $\lambda_i\ge 0$, complementary slackness $\lambda_i g_i(u)=0$, and stationarity $\nabla f+\sum_i\lambda_i\nabla g_i+\sum_j\nu_j\nabla h_j=0$. For convex problems these four conditions are also sufficient — a certificate of global optimality.

  RLHF's objective is exactly such a problem. Fix a prompt $x$ and optimize over the distribution $\pi(\cdot\mid x)$:

  $$
  \max_{\pi}\ \sum_y \pi(y)\,r(x,y)\;-\;\beta\sum_y \pi(y)\log\frac{\pi(y)}{\pi_{\mathrm{ref}}(y\mid x)}
  \quad\text{s.t.}\quad \sum_y\pi(y)=1,\qquad \pi(y)\ge 0 .
  $$

  The negated objective is strictly convex in $\pi$ (KL is strictly convex, the reward term linear), constraints are affine, and $\pi_{\mathrm{ref}}$ itself is strictly feasible, so KKT is necessary and sufficient. With multiplier $\nu$ for normalization and $\lambda_y\ge 0$ for nonnegativity, stationarity in each coordinate $\pi(y)$ gives

  $$
  r(x,y)-\beta\Big(\log\frac{\pi(y)}{\pi_{\mathrm{ref}}(y\mid x)}+1\Big)+\nu+\lambda_y=0
  \;\Longrightarrow\;
  \pi(y)=\pi_{\mathrm{ref}}(y\mid x)\exp\!\Big(\frac{r(x,y)+\nu+\lambda_y}{\beta}-1\Big).
  $$

  The exponential is strictly positive wherever $\pi_{\mathrm{ref}}>0$, so complementary slackness forces $\lambda_y=0$, and $\nu$ is fixed by normalization:

  $$
  \pi^{\star}(y\mid x)=\frac{\pi_{\mathrm{ref}}(y\mid x)\,\exp\!\big(r(x,y)/\beta\big)}{Z(x)},
  \qquad
  Z(x)=\sum_{y'}\pi_{\mathrm{ref}}(y'\mid x)\,\exp\!\big(r(x,y')/\beta\big).
  $$

  Strict concavity makes this the unique global maximizer. Worked example: two candidate responses, $\pi_{\mathrm{ref}}=(0.5,\,0.5)$, rewards $r=(1,\,0)$, $\beta=1$. Unnormalized weights are $(0.5e,\,0.5)\approx(1.359,\,0.500)$, so $Z\approx 1.859$ and $\pi^{\star}\approx(0.731,\,0.269)$ — exactly $\sigma\big((r_1-r_2)/\beta\big)$ for the first response. The limiting cases are diagnostic: $\beta\to\infty$ recovers $\pi_{\mathrm{ref}}$ (the KL term dominates), while $\beta\to 0$ collapses all mass onto $\arg\max_y r(x,y)$ — the degenerate greedy policy whose mode-seeking is what reward hacking exploits.

  This closed form is the mathematical prerequisite for DPO. Inverting it gives $r(x,y)=\beta\log\frac{\pi^{\star}(y\mid x)}{\pi_{\mathrm{ref}}(y\mid x)}+\beta\log Z(x)$; substituting into the Bradley–Terry preference model $P(y_w\succ y_l)=\sigma\big(r(x,y_w)-r(x,y_l)\big)$ cancels the intractable $\log Z(x)$, leaving a loss written purely in policy log-ratios — no reward model, no on-policy sampling. It also yields a falsifiable test for KL-regularized PPO: at convergence, $\beta\log\big(\pi(y\mid x)/\pi_{\mathrm{ref}}(y\mid x)\big)$ must equal $r(x,y)$ up to a per-prompt constant; regressing one on the other over sampled responses measures how far training is from the KKT point.
* **Production Reality & Tradeoffs:** The closed form is exact over the intractable space of all distributions; a finite-capacity network trained on finite samples only approximates it, which is why PPO-trained policies drift from the formula and why DPO's implicit reward can misrank off-distribution responses. Practitioners sweep $\beta$ (commonly around 0.01–0.1 in DPO fine-tunes) to trade reward against KL, monitor the realized KL per token as the health metric, and remember that strong duality arguments assume the true optimum — early stopping, clipping, and minibatch noise all move you off the KKT point.
* **Red Flag:** Writing the Gibbs form without the normalizer and treating $\pi_{\mathrm{ref}}\exp(r/\beta)$ as a usable sampling distribution, or claiming KKT conditions certify optimality for the nonconvex network-weight problem — sufficiency holds in distribution space, not parameter space.
