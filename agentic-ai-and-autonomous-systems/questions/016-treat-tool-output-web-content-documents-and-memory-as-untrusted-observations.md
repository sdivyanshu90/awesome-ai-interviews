### Q: Treat tool output, web content, documents, and memory as untrusted observations.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** External observations may contain prompt injection, stale facts, malformed schemas, secrets, or attacker-controlled instructions. They supply evidence, never authority; trusted orchestration decides what actions are allowed.
* **The Deep Dive:** Represent observations as typed envelopes with source, timestamp, trust level, ACL, content type, and quoted payload. Sanitize active content while retaining protected raw evidence. The model can summarize or propose actions, but executor-side schema, authorization, policy, and invariant checks validate every effect. Memory reads follow the same rule because stored content can be poisoned. Tool errors expose safe codes/details, not credentials or stack dumps. Provenance follows extracted facts into later steps.
* **Production Reality & Tradeoffs:** Labeling content untrusted reduces confusion but is not enforcement. Apply egress control, least privilege, and approval. Test indirect injection through HTML, PDFs, images/OCR, API fields, and prior memory.
Enforce a data/control distinction in the runtime: observations carry source, trust, timestamp, sensitivity, and content fields and cannot create new executable instructions. The planner may extract claims, but policy and tool authorization ignore commands embedded in content. Sanitize rendering, cap size, quarantine active formats, and require provenance-linked evidence. Test nested encodings, retrieved documents that impersonate system messages, and malicious success/error strings from tools.

* **Red Flag:** Telling the model to ignore malicious instructions and then granting broad tool permissions.
