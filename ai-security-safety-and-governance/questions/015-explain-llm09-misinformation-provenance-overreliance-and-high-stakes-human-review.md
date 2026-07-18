### Q: Explain LLM09 Misinformation, provenance, overreliance, and high-stakes human review.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Fluent outputs can be false, stale, or unsupported, and users may overtrust them. Ground claims in traceable evidence, calibrate abstention, communicate limitations, and require qualified review for high-impact decisions.
* **The Deep Dive:** Separate facts from recommendations, cite stable source spans, validate numbers/constraints, detect conflict/freshness, and surface uncertainty. UI design avoids anthropomorphic authority and makes source review easy. High-stakes workflows route to accountable experts with relevant evidence and preserve decisions/audit. Monitor correction and near-miss rates.
* **Production Reality & Tradeoffs:** Citations can be wrong and human review can rubber-stamp. Retrieval reduces but does not eliminate misinformation. Define decision ownership.
Misinformation risk depends on consequence and user reliance. Require provenance-linked claims, distinguish retrieved evidence from generated inference, verify structured facts deterministically, calibrate uncertainty, and abstain when support is insufficient. High-stakes domains need qualified human review with the source—not a checkbox after generation. Measure unsupported-claim and automation-bias outcomes, not just fluency or citation count. UI wording and workflow pressure can make a calibrated model operationally unsafe.

* **Red Flag:** Adding a generic disclaimer while presenting unsupported confident advice.
