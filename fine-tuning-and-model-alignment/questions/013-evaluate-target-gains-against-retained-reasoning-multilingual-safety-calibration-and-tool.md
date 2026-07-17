### Q: Evaluate target gains against retained reasoning, multilingual, safety, calibration, and tool-use capabilities.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** A fine-tune ships only if target improvement exceeds practical thresholds while a retention suite shows no unacceptable regression in general capability, safety, calibration, or tool behavior. Evaluation must be paired against the exact base/reference.
* **The Deep Dive:** Build domain goldens plus paraphrases, adversarial cases, and out-of-domain controls. Measure task quality, refusal precision/recall, factual support, language slices, calibration/selective risk, structured-output validity, tool selection/arguments, and latency. Use paired examples and repeated stochastic trials. Compare SFT/adapter activated versus base and include contamination checks. Plot trade-off frontiers rather than collapsing everything into one score.
* **Production Reality & Tradeoffs:** Benchmarks miss product distribution; canary and shadow logs need privacy controls. A domain gain can come from memorization or verbosity. Define non-regression gates by risk and permit explicit approved trade-offs.
Use paired, bootstrap-resampled deltas against the exact base checkpoint and report worst-slice changes, not only a macro average. Fix decoding and templates to separate capability from sampling. Hard gates cover critical safety/tool regressions; confidence intervals govern noisy preference metrics. Preserve raw generations and adjudication rubrics so a surprising delta can be reproduced rather than explained away by evaluator drift.

* **Red Flag:** Reporting only lower fine-tuning loss or target benchmark gain.
