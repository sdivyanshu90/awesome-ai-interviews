### Q: Compare membership inference, model inversion, training-data extraction, and model stealing.
* **Difficulty:** Senior
* **Category:** Privacy
* **The 10-Second Pitch:** Membership inference asks whether a record trained the model; inversion reconstructs sensitive attributes/examples; extraction elicits memorized data; stealing approximates model behavior/weights through queries.
* **The Deep Dive:** Membership exploits confidence/loss gaps; inversion optimizes inputs or attributes consistent with outputs/gradients; extraction prompts for rare memorized sequences; stealing queries adaptively to train a substitute. Risk grows with overfitting, rare repetitions, exposed logits/embeddings/gradients, and unlimited access. Mitigate with minimization, dedup, regularization/DP where suitable, output restriction, rate/anomaly controls, and legal/operational response.
* **Production Reality & Tradeoffs:** Privacy attacks are distribution- and access-dependent. Hiding logits is insufficient. Differential privacy trades utility and must be correctly accounted.
Membership inference asks whether a record participated in training; inversion reconstructs representative/private attributes; extraction elicits memorized sequences; stealing approximates functionality or weights through queries. Attack success depends on access, overfitting, uniqueness, and query budget. Evaluate with authorized canaries and shadow data, then mitigate through minimization, deduplication, regularization/DP where appropriate, output controls, rate/anomaly limits, and legal/contractual controls. Do not publish sensitive target strings during testing.

* **Red Flag:** Treating all four attacks as prompt injection.
