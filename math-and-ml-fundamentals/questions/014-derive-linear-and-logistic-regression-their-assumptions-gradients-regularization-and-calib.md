### Q: Derive linear and logistic regression, their assumptions, gradients, regularization, and calibration.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Linear regression models a conditional mean with additive noise; logistic regression models Bernoulli log-odds $\log[p/(1-p)]=w^Tx+b$. Their likelihoods yield squared error and BCE, and logistic BCE has gradient $X^T(p-y)/B$.
* **The Deep Dive:** Under $y=Xw+b+\epsilon$, $\epsilon\sim\mathcal N(0,\sigma^2I)$, negative log-likelihood is squared error up to constants. The least-squares normal equation is $X^TXw=X^Ty$, though QR/SVD is numerically preferable. Logistic regression sets $z=Xw+b$, $p=\sigma(z)$ and Bernoulli negative log-likelihood

$$
L=-\frac1B\sum_i\left[y_i\log p_i+(1-y_i)\log(1-p_i)\right].
$$

Because $\partial L_i/\partial z_i=p_i-y_i$,

$$
\nabla_w L=\frac1B X^T(p-y),\qquad \nabla_bL=\frac1B\sum_i(p_i-y_i).
$$

The logistic objective is convex; with full rank and nonseparable data it has a unique optimum. Perfect separation drives unregularized coefficients toward infinity. L2 adds $\lambda\|w\|_2^2/2$ and stabilizes correlated directions; L1 can select sparse features. Multiclass softmax regression generalizes logits and CE. Calibration is an in-distribution asymptotic property under correct specification, not guaranteed with regularization, shift, selection bias, or class weighting.
* **Production Reality & Tradeoffs:** Standardize continuous features, encode categoricals without leakage, handle interactions/nonlinearity explicitly, and tune thresholds from costs rather than 0.5. Class weights change the fitted posterior and require recalibration. Coefficients are conditional associations, not causal effects.
* **Red Flag:** Saying logistic regression predicts a continuous value, applying MSE to probabilities by default, or interpreting $e^{w_j}$ without stating the one-unit/conditional odds assumption.

