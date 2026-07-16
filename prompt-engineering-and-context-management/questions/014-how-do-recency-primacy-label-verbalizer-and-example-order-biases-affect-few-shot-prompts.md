### Q: How do recency, primacy, label, verbalizer, and example-order biases affect few-shot prompts?
* **Difficulty:** Senior
* **Category:** Prompt Engineering
* **The 10-Second Pitch:** Models can overweight early/late examples and token-specific label priors, so measured task accuracy conflates learned mapping with order and verbalizer. Counterbalance, permute, calibrate, and use semantically neutral labels where possible.
* **The Deep Dive:** Recency bias favors examples/labels near the query; primacy favors early anchors/system-like context. If examples are label-blocked, position becomes perfectly correlated with class. Verbalizer bias arises because output tokens such as “yes,” “A,” or rare strings have different pretrained probability, tokenization, and semantic associations. Majority-label and format copying can dominate true task inference.

Create a factorial test: multiple random/Latin-square orderings, label-token permutations, class-balanced sets, and query positions. Report mean/worst-case/variance, flip rate, and calibration. Compare canonical meaningful labels with arbitrary tokens to see whether semantics help or bias. Interleave labels rather than block them; place the most relevant example near the query only if validated. For classification, contextual calibration estimates label probability on content-free inputs and adjusts logits, but does not fix reasoning or distribution shift.

Mechanistically, positional encoding/attention, prompt training distributions, and next-token priors create these effects; one fixed prompt cannot distinguish them.
* **Production Reality & Tradeoffs:** Permutation multiplies evaluation cost and production randomization hurts reproducibility/cache. Choose a robust fixed order after broad testing. Bias varies by model/version/language and must be revalidated.
* **Red Flag:** Reporting best-order accuracy or interpreting label-token preference as model understanding.

