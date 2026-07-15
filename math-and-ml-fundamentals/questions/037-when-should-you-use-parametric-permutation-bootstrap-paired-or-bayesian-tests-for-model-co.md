### Q: When should you use parametric, permutation, bootstrap, paired, or Bayesian tests for model comparison?
* **Difficulty:** Senior
* **Category:** Statistics
* **The 10-Second Pitch:** Exploit paired outcomes whenever models see the same units. Parametric tests gain power under credible distributional models; permutation uses exchangeability; bootstrap estimates effect uncertainty; Bayesian analysis returns posterior decisions under explicit priors and likelihoods.
* **The Deep Dive:** For per-unit differences $d_i$, a paired t-test targets $E[d]$ under approximately well-behaved means; Wilcoxon signed-rank tests a different symmetry/rank hypothesis. McNemar uses discordant pairs for binary correctness. A paired permutation/randomization test swaps model labels/signs within units under the null exchangeability induced by the design. A paired bootstrap resamples units/clusters and recomputes any metric difference, supporting confidence intervals for nonlinear metrics.

Bayesian comparison specifies $p(D\mid\Delta)p(\Delta)$ and reports posterior probability of superiority, practical equivalence, or expected utility. It is not assumption-free; priors and likelihood matter. For online randomized experiments, the randomization unit and interference determine valid inference. For multiple metrics/slices, predeclare primary decisions and control multiplicity. Equivalence/noninferiority needs a practical margin $\delta$, not failure to reject zero difference.
* **Production Reality & Tradeoffs:** Metric distributions can be skewed and benchmark items clustered by source/template. Use cluster bootstrap or hierarchical models. Statistical uncertainty excludes label error and shift. Report effect and interval alongside any decision, and run power analysis for the minimum useful change.
* **Red Flag:** Using independent tests on paired predictions, or choosing whichever test produces significance after seeing results.

