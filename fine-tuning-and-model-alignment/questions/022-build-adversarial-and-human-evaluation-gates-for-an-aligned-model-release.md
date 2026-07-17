### Q: Build adversarial and human evaluation gates for an aligned-model release.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Release gates combine frozen capability/retention sets, adversarial safety and jailbreak tests, expert human pairwise review, tool simulations, and online canaries with explicit rollback thresholds.
* **The Deep Dive:** Create atomic rubrics for helpfulness, honesty, refusal correctness, harmful assistance, bias, privacy, and tool authorization. Include benign-near-policy-boundary cases to measure overrefusal, multilingual and obfuscated attacks, long conversations, indirect injection, and distribution shift. Blind model identity/order, permit ties, adjudicate high-risk disagreement, and calibrate LLM judges to humans. Gate both mean and worst-slice regressions plus latency/cost.
* **Production Reality & Tradeoffs:** Static red-team prompts become training targets. Refresh hidden suites and monitor production incidents. No aggregate score can compensate for a critical policy failure without explicit risk acceptance.
Release gates need severity-aware thresholds fixed before results: a small average win cannot compensate for one reproducible critical exploit. Combine deterministic unit tests, adaptive red teams, blinded domain humans, and representative canary traffic. Bind each result to model, adapter, template, policy, tool, and evaluator versions; retain a tested kill switch and exact rollback artifact.

* **Red Flag:** Shipping on reward-model or automated-judge win rate alone.
