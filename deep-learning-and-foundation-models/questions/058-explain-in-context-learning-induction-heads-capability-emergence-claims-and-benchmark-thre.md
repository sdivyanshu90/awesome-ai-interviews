### Q: Explain in-context learning, induction heads, capability emergence claims, and benchmark threshold artifacts.
* **Difficulty:** Principal
* **Category:** Interpretability
* **The 10-Second Pitch:** In-context learning changes behavior from prompt examples without parameter updates. Induction-head circuits can implement token-pattern continuation, but ICL is broader and distributed. Apparent capability jumps may result from nonlinear/thresholded metrics even when underlying loss improves smoothly.
* **The Deep Dive:** An ICL prompt defines inputs/labels/instructions and the model infers a task or continuation within activations/KV. Hypotheses include implicit Bayesian inference, gradient-descent-like algorithms, retrieval, and learned pattern matching; behavior varies with ordering, labels, formatting, and pretraining distribution. A canonical induction circuit uses one head to identify a previous occurrence/context and another to copy the token that followed it, enabling patterns such as `[A B ... A] -> B`. Causal ablation/path patching supports specific circuits in small models, but large-model ICL uses many heads/MLPs and representations.

“Emergence” depends on metric. Exact-match accuracy may stay zero until continuous probability crosses an argmax threshold, then jump; model families/training compute/data also confound scale. Plot NLL/probability, partial credit, calibration, and multiple scales/seeds. True phase-like changes remain possible but need controlled evidence and preregistered thresholds.

Test ICL with label permutation, counterfactual demonstrations, distractors, order/format changes, and held-out task families.
* **Production Reality & Tradeoffs:** Mechanistic explanations are model/checkpoint-specific and interventions can have off-target effects. Benchmark contamination imitates ICL/capability. Prompt sensitivity is part of reliability, not noise to hide.
* **Red Flag:** Claiming induction heads explain all ICL or inferring a phase transition from one thresholded leaderboard.

