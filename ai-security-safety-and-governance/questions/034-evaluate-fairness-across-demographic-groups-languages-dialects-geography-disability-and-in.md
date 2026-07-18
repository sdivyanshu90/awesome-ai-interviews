### Q: Evaluate fairness across demographic groups, languages, dialects, geography, disability, and intersectional slices.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Define the relevant harm and outcome, collect lawful representative labels, measure quality/calibration/error allocation by groups and intersections, test counterfactuals, and investigate data/system causes.
* **The Deep Dive:** Fairness can concern allocation, quality of service, representation, stereotyping, or accessibility. Report uncertainty because small intersections are noisy. Compare false-positive/negative, calibration, coverage, latency, refusal, and human outcomes. Counterfactual pairs isolate attribute sensitivity but may be unnatural. Language/dialect evaluation uses native experts. Analyze retrieval/data/tool/UI as well as model.
* **Production Reality & Tradeoffs:** Metrics can conflict and protected attributes may be unavailable or sensitive. Avoid inferring them casually. Mitigation may shift harm; monitor after launch.
Define fairness outcomes per task: error, calibration, refusal, harmful compliance, retrieval coverage, latency, or human escalation may matter differently. Evaluate intersectional slices and languages/dialects with adequate sample sizes and confidence intervals; aggregate parity can hide localized failure. Diagnose label, sampling, measurement, and deployment-process bias before choosing mitigation. A single parity metric may conflict with calibration or legitimate prevalence differences, so document the normative choice and affected stakeholders.

* **Red Flag:** Declaring fairness from equal aggregate accuracy.
