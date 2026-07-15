### Q: Compare Monte Carlo, importance sampling, rejection sampling, MCMC, and sequential Monte Carlo.
* **Difficulty:** Senior
* **Category:** Probabilistic Modeling
* **The 10-Second Pitch:** Plain Monte Carlo averages target samples; importance sampling reweights proposal samples; rejection sampling produces independent target draws using a bound; MCMC produces correlated asymptotic samples; SMC maintains weighted particles over a sequence of targets.
* **The Deep Dive:** If $x_i\sim p$, $\hat\mu=N^{-1}\sum f(x_i)$ has standard error $O(N^{-1/2})$. Importance sampling draws $x_i\sim q$ with weights $w_i=p(x_i)/q(x_i)$ and estimates $E_p[f]$ directly or self-normalized; $q$ must cover target support. Weight degeneracy is diagnosed with normalized-weight ESS $1/\sum\tilde w_i^2$.

Rejection sampling needs $p(x)\le Mq(x)$ and accepts with probability $p(x)/(Mq(x))$, so rate is $1/M$ for normalized distributions. MCMC constructs a chain with stationary $p$; Metropolis–Hastings accepts proposals using target/proposal ratios. Burn-in, mixing, multimodal transitions, autocorrelation, and effective sample size determine useful draws. SMC moves particles through time/temperature targets using reweighting, resampling when ESS falls, and mutation kernels; it estimates filtering distributions and normalizing constants.

```text
independent target samples -> Monte Carlo
proposal samples + weights -> importance / SMC
proposal + accept/reject    -> rejection
correlated transition chain -> MCMC
```
* **Production Reality & Tradeoffs:** All methods suffer in high dimension without structure. Unnormalized targets support MCMC/IS ratios but not every estimator. Report Monte Carlo error and convergence diagnostics; multiple chains are necessary but not sufficient. Resampling introduces dependence and particle impoverishment.
* **Red Flag:** Treating MCMC draws as IID or trusting importance sampling when a few weights dominate.

