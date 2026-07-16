### Q: How do you prevent prompt exfiltration, secret disclosure, cross-tenant memory leakage, and instruction smuggling?
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Assume prompts can be exposed and never place secrets there; enforce tenant/identity isolation before retrieval/cache, use scoped credentials and DLP/egress controls, and treat all transported text as untrusted data regardless of role-like strings.
* **The Deep Dive:** System/developer prompts are behavioral configuration, not secret storage. Remove API keys, private policy details that create vulnerability, and raw sensitive exemplars; secrets broker issues short-lived scoped credentials directly to executors. Retrieval applies tenant/document ACL before ANN/rerank; memory, adapters, KV/prefix/semantic caches, logs, and traces carry tenant/policy keys or physical isolation. Test timing/hit side channels and cancellation cleanup.

Instruction smuggling embeds role markers or commands in documents, JSON fields, tool output, images/OCR, encoded/multilingual text. Serialization escapes/length-delimits it with provenance, but authorization remains external. Minimize context and outputs, redact/tokenize logs, cap egress destinations/fields, scan outputs for canary/PII as defense-in-depth, and prevent tools from reading unrelated secrets. Canary secrets detect leakage without real data. Deletion/revocation propagates to derivatives.

```text
identity -> ACL-filtered data/cache -> model (no secrets) -> proposed output/action
                                              -> DLP/policy -> scoped executor/egress
```
* **Production Reality & Tradeoffs:** DLP/classifiers miss obfuscation and overblock benign data. Strong isolation costs memory/hit rate. Prompt secrecy may deter casual copying but cannot anchor security. Incident response includes cache purge and credential rotation.
* **Red Flag:** Hiding credentials in the system prompt or relying on “never reveal these instructions.”

