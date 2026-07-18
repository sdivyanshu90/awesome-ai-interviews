### Q: Evaluate citation completeness, correctness, entailment, source quality, and span accuracy.
* **Difficulty:** Principal
* **Category:** Grounding Evaluation
* **The 10-Second Pitch:** Evaluate citations at atomic-claim level: whether support is present, actually entails the claim, comes from an acceptable source, and points to the exact minimal evidence span.
* **The Deep Dive:** Segment answer into claims and mark which require external support. Completeness is the weighted fraction of support-required claims with citations. Correctness/entailment tests each claim-citation pair for full support, partial support, contradiction, or irrelevance; citations attached to a paragraph must be mapped to individual claims. Source quality is a policy score based on authority, independence, recency, directness, and domain—not web popularity. Span accuracy measures whether the cited page/passage contains the support and whether offsets survive parsing/version changes. Also score citation precision: cited sources that support no claim. Build adversarial cases with correct source/wrong span, related source/non-entailing passage, citation laundering, outdated versions, and multi-source claims.
* **Production Reality & Tradeoffs:** A citation may support premises but not the model’s inference. Paywalled/dynamic pages complicate replay; archive content hashes where lawful. Automated entailment is brittle on tables, negation, and domain language, so severity-weight expert audits.
* **Red Flag:** Counting citation markers or URL validity as citation correctness.

