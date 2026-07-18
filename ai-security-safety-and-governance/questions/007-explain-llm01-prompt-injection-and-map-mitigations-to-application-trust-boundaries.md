### Q: Explain LLM01 Prompt Injection and map mitigations to application trust boundaries.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** LLM01 is unintended behavior caused by attacker-controlled instructions in model input, direct or indirect. Mitigations reduce impact by isolating privileges and validating effects; they do not make the model perfectly instruction-secure.
* **The Deep Dive:** Map untrusted sources entering context, model outputs crossing into renderers/tools, and credentials/data accessible downstream. Apply provenance/data separation at context, minimal retrieval, tool capability scoping, action validation, approvals, output encoding, and monitoring. Restrict egress so sensitive context cannot be sent outward. Test delayed/stored and multimodal payloads.
* **Production Reality & Tradeoffs:** RAG and fine-tuning do not eliminate injection. Input filtering alone is bypassable. Security is determined by achievable effects, not whether the model repeats a phrase.
LLM01 is not fixed by a stronger system prompt. Identify every attacker-controlled context source and every downstream effect. Mitigations are boundary-specific: provenance for retrieval, data-only rendering for tool output, constrained action schemas, independent authorization, sandboxing, and approval for irreversible effects. If a model can read a document and hold a privileged credential in the same decision, assume the document can attempt to wield that credential.

* **Red Flag:** Defining injection only as users overriding the system prompt.
