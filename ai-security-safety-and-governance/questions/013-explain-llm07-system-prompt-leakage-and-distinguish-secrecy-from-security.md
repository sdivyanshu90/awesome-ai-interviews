### Q: Explain LLM07 System Prompt Leakage and distinguish secrecy from security.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** System instructions may be exposed through outputs, errors, or behavior and should not contain secrets. Their confidentiality can protect intellectual property, but security cannot depend on hiding them.
* **The Deep Dive:** Remove credentials/private data from prompts; retrieve policy from controlled stores as needed. Limit verbose errors and indirect reflection. Treat prompt templates as potentially discoverable and enforce permissions outside. If prompt confidentiality matters, access-control source repositories/logs and monitor extraction attempts, but assume attackers infer constraints.
* **Production Reality & Tradeoffs:** Blocking prompt quotation can harm legitimate debugging and is bypassable. Prompt leakage differs from sensitive user data leakage but can facilitate attacks.
Prompt leakage is often undesirable, but confidentiality cannot rely on the model refusing to repeat context. Keep secrets and security-critical logic outside prompts; expose opaque handles and brokered capabilities instead. Assume templates may be inferred through behavior or exfiltrated through tools/logs. Redact diagnostics and restrict prompt observability by role. A leaked policy prompt should not grant access or make bypassing deterministic enforcement possible.

* **Red Flag:** Claiming the system prompt is a secure secret boundary.
