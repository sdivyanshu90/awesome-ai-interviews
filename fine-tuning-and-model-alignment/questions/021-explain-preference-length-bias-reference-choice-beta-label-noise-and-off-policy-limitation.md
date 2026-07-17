### Q: Explain preference length bias, reference choice, beta, label noise, and off-policy limitations.
* **Difficulty:** Principal
* **Category:** Alignment
* **The 10-Second Pitch:** Preference optimization is sensitive to response length and dataset/policy mismatch. Reference and beta define allowable drift; noisy or ambiguous labels distort gradients; offline pairs cover only behaviors sampled by earlier policies.
* **The Deep Dive:** Sequence log probabilities sum token terms, often penalizing longer responses; length normalization changes the objective and can create other bias. A strong reference anchors behavior, while a mismatched reference changes implicit rewards. In DPO-like losses, beta scales reference-relative preference separation. Annotator noise, ties, and inconsistent criteria need robust losses/filtering or probabilistic labels. As policy moves beyond pair support, offline objectives cannot evaluate unseen behaviors.
* **Production Reality & Tradeoffs:** Track response length, KL, diversity, win rate by slice, and independent human quality. Deduplicate prompts/responses and avoid data leakage. Tune beta jointly with LR/epochs.
Diagnose length bias using matched-content verbosity perturbations and a regression of preference on response length. Compute all four policy/reference log-probability terms with identical templates and response masks. Since $\beta$ depends on whether log probabilities are summed or length-normalized, quote the convention with every result. Off-support pairs produce extreme noisy gradients and may need filtering or conservative weighting.

* **Red Flag:** Treating beta as ordinary softmax temperature or ignoring pair-generation policy.
