### Q: Derive entropy, cross-entropy, KL divergence, Jensen–Shannon divergence, and mutual information.
* **Difficulty:** Senior
* **Category:** Information Theory
* **The 10-Second Pitch:** Entropy is expected self-information, cross-entropy codes $P$ with $Q$, KL is the excess cross-entropy over optimal coding, JS is a finite symmetric mixture of KLs, and mutual information is KL between a joint and independent marginals.
* **The Deep Dive:** For discrete $P$,

$$
H(P)=-\sum_xp(x)\log p(x),\quad
H(P,Q)=-\sum_xp(x)\log q(x),
$$

and

$$
D_{KL}(P\|Q)=\sum_xp(x)\log\frac{p(x)}{q(x)}=H(P,Q)-H(P)\ge0.
$$

KL is not symmetric and can be infinite if $Q=0$ where $P>0$. With $M=(P+Q)/2$, $JS(P,Q)=\frac12KL(P\|M)+\frac12KL(Q\|M)$ is symmetric and bounded (by $\log2$ in nats for two distributions); $\sqrt{JS}$ is a metric under standard definitions.

Mutual information

$$
I(X;Y)=D_{KL}(p(x,y)\|p(x)p(y))=H(X)-H(X\mid Y)
$$

measures dependence and is zero iff independent. Cross-entropy training minimizes forward KL to the data distribution up to fixed $H(P)$. Continuous differential entropy can be negative and changes under reparameterization, while KL/MI remain invariant under suitable bijections.
* **Production Reality & Tradeoffs:** Estimating entropy/MI in high dimensions is difficult; variational bounds and contrastive estimators can be loose and batch-dependent. Log base sets bits versus nats. Low empirical CE may reflect contamination or miss rare modes.
* **Red Flag:** Calling KL a distance or comparing discrete entropy directly with differential entropy without acknowledging the distinction.

