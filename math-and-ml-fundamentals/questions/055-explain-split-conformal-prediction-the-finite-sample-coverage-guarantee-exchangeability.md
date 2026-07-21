### Q: Explain split conformal prediction: the finite-sample coverage guarantee, exchangeability, and failure under distribution shift.
* **Difficulty:** Senior
* **Category:** Statistics
* **The 10-Second Pitch:** Score a held-out calibration set with a nonconformity function, take the $\lceil(n+1)(1-\alpha)\rceil$-th smallest score as threshold $\hat q$, and predict the set of labels scoring at most $\hat q$. Exchangeability makes the test score's rank uniform among the $n+1$ scores, giving marginal coverage between $1-\alpha$ and $1-\alpha+\frac{1}{n+1}$ for any model and any distribution — a guarantee that is finite-sample, distribution-free, and dead the moment shift breaks exchangeability.
* **The Deep Dive:** Split the data into a training set that fits the model $f$ and a calibration set of $n$ labeled points on which you compute nonconformity scores $s_i=s(x_i,y_i)$ — for regression typically $|y_i-f(x_i)|$, for classification $1-\hat p(y_i\mid x_i)$. The prediction set for a new input is

  $$
  C(x)=\{y: s(x,y)\le\hat q\},
  \qquad
  \hat q=s_{(k)},\quad k=\big\lceil(n+1)(1-\alpha)\big\rceil,
  $$

  where $s_{(k)}$ is the $k$-th order statistic. The guarantee needs only that the $n+1$ scores (calibration plus test) are exchangeable — any permutation of them is equally likely, which holds under i.i.d. sampling. Exchangeability makes the test score's rank uniform on $\{1,\dots,n+1\}$, so

  $$
  \mathbb{P}\big(s_{n+1}\le s_{(k)}\big)=\frac{k}{n+1}=\frac{\lceil(n+1)(1-\alpha)\rceil}{n+1}\;\ge\;1-\alpha,
  $$

  and coverage $\mathbb{P}\big(y_{n+1}\in C(x_{n+1})\big)$ inherits the same bound; with continuous scores it is also at most $1-\alpha+\frac{1}{n+1}$. Note the model quality never enters — a bad $f$ costs you wide sets, never coverage.

  Worked example with $\alpha=0.1$: with $n=99$ calibration residuals, $k=\lceil 100\times 0.9\rceil=90$, so $\hat q$ is the 90th-smallest residual — say $2.3$ — and every prediction becomes $f(x)\pm 2.3$ with coverage guaranteed in $[0.900,\ 0.910]$. Shrink to $n=9$: $k=\lceil 10\times 0.9\rceil=9$ forces $\hat q$ to be the largest residual, so the guarantee still holds but the sets are conservative — coverage can sit anywhere in $[0.9,\ 1.0]$. As $n\to\infty$ the sandwich closes and coverage converges to exactly $1-\alpha$; that is the limiting case. The guarantee is also strictly marginal: it averages over draws of calibration and test data, so a model can cover 95% of one subgroup and 80% of another while the average stays at 90%.

  ```text
  train ------> fit f
  calib (n) --> scores s_1..s_n --sort--> q_hat = s_(k),  k = ceil((n+1)(1-a))
  test x -----> C(x) = { y : s(x,y) <= q_hat }
  exchangeable scores  => rank(s_test) uniform on {1..n+1} => coverage >= 1-a
  shifted test scores  => stochastically larger => rank piles up high => coverage < 1-a
  ```

  Distribution shift attacks exactly the uniform-rank step, as the last line shows. Under covariate or label shift, test scores are typically stochastically larger than calibration scores, the test rank concentrates near $n+1$, and realized coverage drops below $1-\alpha$ with no warning from the math. Mitigations restore a corrected form of exchangeability: weighted conformal reweights calibration scores by likelihood ratios $w(x)=\frac{dP_{\text{test}}}{dP_{\text{train}}}(x)$ when the shift is estimable; adaptive conformal inference runs the online update $q_{t+1}=q_t+\gamma(\mathrm{err}_t-\alpha)$, which tracks $\alpha$ miscoverage on average under arbitrary drift; Mondrian (group-conditional) calibration computes $\hat q$ per group to fix subgroup undercoverage. The falsifiable test: hold out $m=500$ fresh labeled points; under exchangeability covered counts are Binomial with mean $450$ and standard deviation $\sqrt{500\times 0.9\times 0.1}\approx 6.7$, so observing $410$ covered ($82\%$, about six standard deviations low) rejects exchangeability decisively.
* **Production Reality & Tradeoffs:** Calibration spends labeled data and must be refreshed on the cadence at which the world drifts; the score function decides usefulness — naive scores give valid but uninformatively large sets, which is why APS/RAPS exist for classification and conformalized quantile regression for heteroscedastic targets. Monitor rolling empirical coverage as the primary alarm, expect set sizes to blow up exactly when the model is uncertain (that is the feature), and never promise per-input probabilities to downstream consumers — the contract is marginal.
* **Red Flag:** Reading the guarantee as conditional — claiming $\mathbb{P}(y\in C(x)\mid x)\ge 1-\alpha$ for each input — or asserting it needs Gaussian residuals or survives distribution shift; it needs neither Gaussianity nor a good model, but it does need exchangeability.
