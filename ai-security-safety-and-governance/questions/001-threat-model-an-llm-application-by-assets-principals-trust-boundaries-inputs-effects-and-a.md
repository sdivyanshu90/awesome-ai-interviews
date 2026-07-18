### Q: Threat-model an LLM application by assets, principals, trust boundaries, inputs, effects, and attackers.
* **Difficulty:** Senior
* **Category:** Security
* **The 10-Second Pitch:** Enumerate what must be protected, who acts, where trust changes, which inputs are attacker-controlled, and what effects are possible. Then map abuse paths and enforce controls outside the model.
* **The Deep Dive:** Start with assets and security properties: tenant data/confidentiality, tool effects/integrity, credentials, prompts and weights, audit evidence, budget/availability, and user trust. Identify principals separately—end user, tenant administrator, service operator, model provider, agent runtime, tool service, data owner, and external content author—because their authority differs. Draw each process, store, data flow, and trust transition. Inputs include prompt text, images/audio, retrieved documents, web pages, tool output, memory, model artifacts, configuration, and telemetry; any may be malicious even when fetched by a trusted service.

```text
untrusted client ----> API/auth ----> orchestrator ----> model provider
                           |                |
                    tenant boundary        +----> retrieval ----> indexed external content
                                            |
                                            +----> policy enforcement ----> scoped tool ----> side effect
                                                    ^                         |
                                                    +------ audit/result <----+
```

  For every flow, enumerate spoofing, tampering, repudiation, disclosure, denial of service, and elevation of privilege, then add ML-specific poisoning, extraction, evasion, inversion, and unsafe overreliance. Express abuse as a causal path—attacker capability, entry point, violated assumption, reachable effect, and impact. Example: a document author stores an indirect instruction; retrieval crosses it into model context; the model proposes a payment; an executor incorrectly treats the model as an authorization principal. Controls belong at the failed boundary: document provenance/ACL, least-privilege tool surface, deterministic authorization, immutable approval, egress policy, and audit. Score residual risk after controls, assign owners, and specify detection/containment.
* **Production Reality & Tradeoffs:** The model is an untrusted decision aid, not a security principal or enforcement boundary. Threat models decay whenever autonomy, tools, providers, indexes, memory, modalities, or cache sharing changes; tie review to architecture and release changes. Validate controls with abuse-case tests and incident exercises, including combined failures. Prompt filters can reduce attempts but cannot compensate for broad credentials or missing authorization.
* **Red Flag:** Listing jailbreak prompts without assets, effects, or authorization paths.
