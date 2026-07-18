### Q: Why can prompt hierarchy and delimiters reduce but not eliminate injection?
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Hierarchy tells the model which instructions should dominate and delimiters mark data, but all content is still processed by one probabilistic model. Neither creates an enforceable isolation boundary.
* **The Deep Dive:** A malicious document can describe higher-priority-looking commands, request secret transformation, or exploit learned patterns despite labels. Role metadata is stronger than literal `SYSTEM:` text but compliance is statistical. Delimiters can be escaped, mimicked, or semantically overridden. Critical authorization, tool allowlists, data access, and output handling must live in deterministic components; the model may only propose within capabilities.
* **Production Reality & Tradeoffs:** Hierarchy is still valuable for reliability. Defense in depth includes provenance, minimal context, least privilege, validation, approval, monitoring, and safe failure.
Hierarchy is learned behavior, not a cryptographic isolation boundary. Delimiters improve parsing but the same model still jointly processes control and attacker text, and an instruction-like document may win through salience or generalization. Treat hierarchy as one probabilistic layer. Deterministic code must enforce authorization, schema constraints, data provenance, and side-effect approval even when the model reports that a lower-priority instruction is trustworthy.

* **Red Flag:** Claiming XML tags or a stronger system message solves injection.
