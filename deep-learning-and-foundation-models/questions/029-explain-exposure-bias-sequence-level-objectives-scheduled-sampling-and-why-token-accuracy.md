### Q: Explain exposure bias, sequence-level objectives, scheduled sampling, and why token accuracy is misleading.
* **Difficulty:** Senior
* **Category:** Training
* **The 10-Second Pitch:** Teacher-forced MLE trains on data prefixes while generation visits model prefixes; early errors can shift future states. Token accuracy ignores probability quality and valid alternative continuations. Sequence objectives target outcomes but introduce search, variance, and credit-assignment problems.
* **The Deep Dive:** MLE is statistically consistent under ideal data/model assumptions, so exposure bias is not simply “teacher forcing is wrong.” Under finite/misspecified models and decoding, sampled errors create prefixes absent from training and can compound. Scheduled sampling sometimes replaces ground-truth previous tokens with model samples, but creates an inconsistent objective and ambiguous target conditioning. Professor-forcing/adversarial state matching and data aggregation are alternatives with their own issues.

Sequence-level training may minimize expected task loss using policy gradients $E[R\nabla\log p(y)]$, minimum-risk training over sampled candidates, beam-based losses, or preference optimization. These align with BLEU/task/reward but suffer high variance, biased candidate sets, sparse credit, and reward gaming; a strong MLE/SFT initialization is typical.

Token accuracy marks one reference token correct even when synonyms/multiple continuations are valid, ignores confidence, and weights unimportant tokens equally. Use NLL/calibration plus sequence task success, factuality, constraints, and human evaluation.
* **Production Reality & Tradeoffs:** On-policy sampling is expensive and can destabilize language quality. Scheduled methods change batching and reproducibility. Analyze error recovery by injecting bad prefixes and evaluating continuation, not only aggregate exact match.
* **Red Flag:** Claiming exposure bias proves MLE should be abandoned, or using next-token accuracy as generation quality.

