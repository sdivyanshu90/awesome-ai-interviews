### Q: Compare zero-shot, one-shot, few-shot, and many-shot prompting.
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** They differ in demonstration count and therefore prior/task ambiguity, coverage, token cost, and sensitivity. More examples help only when labels and formatting are representative, consistent, and selected under a measured budget.
* **The Deep Dive:** Zero-shot relies on instruction and pretrained task knowledge; it is cheap and avoids example leakage but may misinterpret label semantics. One-shot anchors format and one mapping yet can overfit/copy that example. Few-shot covers labels, boundaries, and missing/refusal behavior; selection and order become important. Many-shot can teach complex mappings and average ambiguity, but consumes context, increases prefill/cost, distractors, contradictions, and recency effects.

Compare at fixed output budget and exact serialization. Use stratified examples, query-similar retrieval plus diversity, consistent schema, and no test-answer leakage. Plot quality versus demonstration count rather than assuming monotonicity; ablate individual examples, permute order/labels, and test semantic counterfactuals. Static demonstrations maximize reproducibility and prefix-cache hits; dynamic examples improve query fit but require safe provenance, ACLs, and injection handling.

```text
zero: instruction -> query
one: instruction -> example -> query
few/many: instruction -> balanced demonstrations -> query
```
* **Production Reality & Tradeoffs:** Demonstrations are production data with privacy, contamination, and maintenance costs. Weak models may copy surface patterns; strong models may need none. Long examples reduce space for evidence/output and can trigger lost-in-middle.
* **Red Flag:** Equating more examples with better performance or selecting demonstrations using the held-out answer.

