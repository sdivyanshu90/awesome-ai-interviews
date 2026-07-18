### Q: Balance safety false positives, false negatives, disparate impact, and useful safe completion.
* **Difficulty:** Principal
* **Category:** Safety
* **The 10-Second Pitch:** Choose thresholds/actions from risk-weighted costs, not maximum refusal. Measure harmful misses, benign blocks, quality of safe alternatives, and subgroup disparities under calibrated review.
* **The Deep Dive:** Build labeled boundary sets and production sampling with expert adjudication. Plot precision-recall and expected harm/utility across thresholds. Use tiered actions: allow, transform/safe-complete, warn, require review, or block. Slice by language/dialect/topic/group because error rates differ. Appeals and correction data expose overblocking. High-impact categories tolerate different trade-offs than low-risk content.
* **Production Reality & Tradeoffs:** Ground truth is contested and evolves. Optimizing aggregate safety can burden minorities or accessibility. Document decision ownership and monitor drift.
Choose thresholds from expected harm, not overall accuracy. Estimate severity-weighted false negatives and the user/coverage cost of false positives by group and intent. Provide safe completion when details can be withheld without refusing the legitimate goal. Calibration and subgroup confidence intervals matter because small slices create noisy disparities. Monitor appeals and downstream behavior, but correct selection bias in who appeals. Hard legal/safety constraints remain constraints even if aggregate utility favors violation.

* **Red Flag:** Reporting refusal rate as safety quality.
