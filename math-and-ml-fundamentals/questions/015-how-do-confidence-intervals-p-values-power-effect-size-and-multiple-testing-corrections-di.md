### Q: How do confidence intervals, p-values, power, effect size, and multiple-testing corrections differ?
* **Difficulty:** Senior
* **Category:** Statistics
* **The 10-Second Pitch:** Effect size is the practical difference; a confidence interval quantifies estimator uncertainty; a p-value measures null-model incompatibility; power is detection probability for a prespecified effect; multiplicity procedures control error across a family of tests.
* **The Deep Dive:** A 95% frequentist confidence procedure contains the fixed true parameter in 95% of repeated samples; it is not a 95% posterior probability for this realized interval. A p-value is $P(T\ge T_{obs}\mid H_0)$ under the test’s assumptions, not $P(H_0\mid D)$ and not effect importance. Power $1-\beta=P(\text{reject }H_0\mid\text{specified alternative})$ depends on effect, variance, sample size, design, and alpha. Plan around a minimum practically important difference and report the estimate with interval.

Testing $m$ hypotheses at level $\alpha$ inflates false positives. Bonferroni/Holm control family-wise error; Benjamini–Hochberg controls false discovery rate under assumptions. Paired model outputs need paired tests/bootstrap; repeated users need cluster-aware inference. Noninferiority/equivalence requires prespecified margins and is not established by failing to reject difference. Repeated peeking needs group-sequential or always-valid methods.
* **Production Reality & Tradeoffs:** Statistical intervals cover sampling variability, not benchmark contamination, label bias, or distribution shift. Huge datasets make trivial effects significant; rare severe errors can have wide intervals even with many easy examples. Preregister confirmatory metrics/slices and separate exploration.
* **Red Flag:** Reporting $p<0.05$ as a 95% chance the model is better, or claiming models are equal because a test is nonsignificant.

