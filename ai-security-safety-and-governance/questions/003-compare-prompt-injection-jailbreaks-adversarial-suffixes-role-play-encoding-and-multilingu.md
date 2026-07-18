### Q: Compare prompt injection, jailbreaks, adversarial suffixes, role-play, encoding, and multilingual attacks.
* **Difficulty:** Senior
* **Category:** Security
* **The 10-Second Pitch:** Prompt injection targets application instruction integrity; jailbreaks aim to bypass model policy. Suffixes, role-play, encodings, and language shifts are attack techniques that can serve either goal.
* **The Deep Dive:** Adversarial suffixes optimize token sequences for target behavior; role-play reframes intent; encoding hides semantics; multilingual/code-switching exploits uneven policy coverage. Injection becomes dangerous when the application gives the model authority or sensitive context. Jailbreak evaluation measures harmful output; injection evaluation must measure unauthorized data/action effects. Normalize carefully, classify intent/output, and enforce application permissions independently.
* **Production Reality & Tradeoffs:** Blocking strings creates easy bypasses and false positives. Models and attacks evolve, requiring continuous suites and production monitoring.
A jailbreak tries to elicit policy-prohibited model output; prompt injection tries to subvert application instructions or authority. Adversarial suffixes optimize token patterns, role-play changes framing, encodings evade filters, and multilingual attacks exploit uneven policy coverage. The categories overlap, so evaluate by achieved effect—secret disclosure, unsafe content, unauthorized tool use—not attack label. Decode/normalize cautiously because recursive decoding itself can create parser differentials.

* **Red Flag:** Using jailbreak refusal rate as the sole application-security metric.
