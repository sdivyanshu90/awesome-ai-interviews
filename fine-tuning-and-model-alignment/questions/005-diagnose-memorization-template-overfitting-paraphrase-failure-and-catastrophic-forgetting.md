### Q: Diagnose memorization, template overfitting, paraphrase failure, and catastrophic forgetting.
* **Difficulty:** Senior
* **Category:** Training
* **The 10-Second Pitch:** Catastrophic forgetting is loss of prior capabilities after adapting to a narrow distribution. Measure both target gains and retained base capabilities; mitigate with lower learning rates, fewer steps, adapters, replay/general-mixture data, regularization, and early stopping.
* **The Deep Dive:** Gradient updates for a specialized corpus can move shared representations away from general-language, safety, multilingual, or reasoning behavior. A held-out retention suite makes regressions visible. Replay mixes general data; KL-to-base/reference penalties constrain drift; adapters isolate changes but can still alter behavior when activated.
Separate memorization, template overfitting, narrow adaptation, and forgetting. Measure canary extraction, train-to-paraphrase gaps, likelihood shifts, and a vector of retained-capability deltas. Replay and KL penalties trade target gain for retention; they do not guarantee it. Select checkpoints on a Pareto curve of target gain versus worst-slice regression, and inspect layer update norms. Adapters isolate parameters but can still degrade behavior when activated.

* **Production Reality & Tradeoffs:** Retention tests must include the exact capabilities users rely on, not generic leaderboards alone. Overconstraining to base behavior can prevent necessary adaptation.
* **Red Flag:** Evaluating a fine-tune only on its training-domain benchmark.
