### Q: Explain the law of large numbers, central limit theorem, bootstrap, and concentration bounds.
* **Difficulty:** Senior
* **Category:** Statistics
* **The 10-Second Pitch:** LLN gives convergence of averages, CLT gives an asymptotic fluctuation distribution, bootstrap estimates sampling distributions from resampling, and concentration bounds give finite-sample tail guarantees. Each needs different independence, moment, and regularity assumptions.
* **The Deep Dive:** For IID variables with finite mean, $\bar X_n\to\mu$ in probability/almost surely under standard LLNs. With finite nonzero variance, the CLT states

$$
\sqrt n\,\frac{\bar X_n-\mu}{\sigma}\Rightarrow\mathcal N(0,1).
$$

It does not say the raw data become Gaussian, and convergence can be poor for heavy tails/rare events. The nonparametric bootstrap samples $n$ observed units with replacement and recomputes a statistic; percentile/BCa/studentized intervals require the statistic and empirical distribution to behave suitably. Resample clusters or time blocks when observations depend.

Hoeffding gives exponential bounds for independent bounded variables; Bernstein incorporates variance and can be tighter; sub-Gaussian/sub-exponential assumptions yield other bounds. Bounds are often conservative but finite-sample and distribution-robust within assumptions. Berry–Esseen quantifies CLT approximation using the third absolute moment. Rare-event rates may need exact/binomial or importance-sampling methods.
* **Production Reality & Tradeoffs:** More data does not remove sampling bias, distribution shift, or dependence. Ordinary bootstrap fails for extrema, tiny samples, and some nonsmooth statistics. Always match the resampling unit to deployment and report assumptions.
* **Red Flag:** Saying the CLT makes any dataset normal, or bootstrapping individual rows when users contribute repeated correlated rows.

