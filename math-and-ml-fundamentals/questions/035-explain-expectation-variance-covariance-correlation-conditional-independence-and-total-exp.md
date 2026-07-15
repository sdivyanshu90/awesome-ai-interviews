### Q: Explain expectation, variance, covariance, correlation, conditional independence, and total expectation/variance.
* **Difficulty:** Mid
* **Category:** Probability
* **The 10-Second Pitch:** Expectation averages under a distribution; variance is expected squared deviation; covariance captures unnormalized linear co-movement; correlation normalizes it. Conditional independence is a factorization statement, and total expectation/variance decompose uncertainty across conditioning groups.
* **The Deep Dive:** $E[X]=\int x\,dP(x)$ and $\operatorname{Var}(X)=E[(X-E[X])^2]=E[X^2]-E[X]^2$. Covariance is $E[(X-\mu_X)(Y-\mu_Y)]$; correlation divides by $\sigma_X\sigma_Y$ and is undefined if either variance is zero. Zero covariance does not imply independence except in special families such as jointly Gaussian variables.

Conditional independence $X\perp Y\mid Z$ means

$$
p(x,y\mid z)=p(x\mid z)p(y\mid z)
$$

for relevant $z$. Marginal independence may disappear after conditioning and vice versa (confounding/collider effects). The tower property and law of total variance are

$$
E[X]=E[E[X\mid Z]],\qquad
\operatorname{Var}(X)=E[\operatorname{Var}(X\mid Z)]+\operatorname{Var}(E[X\mid Z]).
$$

The first variance term is average within-group noise; the second is between-group variation. For vectors, covariance is PSD and transforms as $\operatorname{Cov}(AX)=A\Sigma A^T$.
* **Production Reality & Tradeoffs:** Sample estimates are sensitive to outliers and dependence; use cluster/time-aware uncertainty. Correlation changes under selection and does not establish causation. Numerically compute variance with stable algorithms rather than subtracting two large close moments.
* **Red Flag:** Saying uncorrelated means independent, or interpreting conditional independence without naming the conditioning set.

