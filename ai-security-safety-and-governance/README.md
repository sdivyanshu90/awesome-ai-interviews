# 8. AI Security, Safety & Governance

Thirty-six interview profiles covering adversarial prompting, the OWASP 2025 LLM risk taxonomy, poisoning and privacy, agent/runtime security, safety operations, and governance. Every question has its own answer file.

[Back to the complete syllabus](../SYLLABUS.md)

## Threat modeling and prompt attacks

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 001 | [Threat-model an LLM application by assets, principals, trust boundaries, inputs, effects, and attackers.](questions/001-threat-model-an-llm-application-by-assets-principals-trust-boundaries-inputs-effects-and-a.md) | Senior | Security |
| 002 | [Compare direct, indirect, stored, cross-domain, multimodal, and tool-result prompt injection.](questions/002-compare-direct-indirect-stored-cross-domain-multimodal-and-tool-result-prompt-injection.md) | Senior | Security |
| 003 | [Compare prompt injection, jailbreaks, adversarial suffixes, role-play, encoding, and multilingual attacks.](questions/003-compare-prompt-injection-jailbreaks-adversarial-suffixes-role-play-encoding-and-multilingu.md) | Senior | Security |
| 004 | [Why can prompt hierarchy and delimiters reduce but not eliminate injection?](questions/004-why-can-prompt-hierarchy-and-delimiters-reduce-but-not-eliminate-injection.md) | Principal | Security |
| 005 | [Design defense in depth using instruction/data separation, provenance, least privilege, validation, and approval.](questions/005-design-defense-in-depth-using-instruction-data-separation-provenance-least-privilege-valid.md) | Principal | Architecture |
| 006 | [Evaluate injection and jailbreak robustness without turning a static benchmark into a false guarantee.](questions/006-evaluate-injection-and-jailbreak-robustness-without-turning-a-static-benchmark-into-a-fals.md) | Principal | Evaluation |

## OWASP Top 10 for LLM Applications (2025)

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 007 | [Explain LLM01 Prompt Injection and map mitigations to application trust boundaries.](questions/007-explain-llm01-prompt-injection-and-map-mitigations-to-application-trust-boundaries.md) | Senior | OWASP |
| 008 | [Explain LLM02 Sensitive Information Disclosure across prompts, outputs, caches, logs, and training data.](questions/008-explain-llm02-sensitive-information-disclosure-across-prompts-outputs-caches-logs-and-trai.md) | Senior | OWASP |
| 009 | [Explain LLM03 Supply Chain risk for weights, datasets, packages, containers, plugins, and endpoints.](questions/009-explain-llm03-supply-chain-risk-for-weights-datasets-packages-containers-plugins-and-endpo.md) | Senior | OWASP |
| 010 | [Explain LLM04 Data and Model Poisoning across pretraining, tuning, preference, RAG, and memory.](questions/010-explain-llm04-data-and-model-poisoning-across-pretraining-tuning-preference-rag-and-memory.md) | Senior | OWASP |
| 011 | [Explain LLM05 Improper Output Handling leading to SQL, shell, HTML, SSRF, or code injection.](questions/011-explain-llm05-improper-output-handling-leading-to-sql-shell-html-ssrf-or-code-injection.md) | Senior | OWASP |
| 012 | [Explain LLM06 Excessive Agency and enforce authorization independently of the model.](questions/012-explain-llm06-excessive-agency-and-enforce-authorization-independently-of-the-model.md) | Senior | OWASP |
| 013 | [Explain LLM07 System Prompt Leakage and distinguish secrecy from security.](questions/013-explain-llm07-system-prompt-leakage-and-distinguish-secrecy-from-security.md) | Senior | OWASP |
| 014 | [Explain LLM08 Vector and Embedding Weaknesses including poisoning, inversion, and ACL failure.](questions/014-explain-llm08-vector-and-embedding-weaknesses-including-poisoning-inversion-and-acl-failur.md) | Senior | OWASP |
| 015 | [Explain LLM09 Misinformation, provenance, overreliance, and high-stakes human review.](questions/015-explain-llm09-misinformation-provenance-overreliance-and-high-stakes-human-review.md) | Senior | OWASP |
| 016 | [Explain LLM10 Unbounded Consumption, denial of wallet, model DoS, and resource controls.](questions/016-explain-llm10-unbounded-consumption-denial-of-wallet-model-dos-and-resource-controls.md) | Senior | OWASP |

## Poisoning, privacy, and supply chain

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 017 | [Compare poisoning, backdoors, trigger attacks, clean-label attacks, and availability attacks.](questions/017-compare-poisoning-backdoors-trigger-attacks-clean-label-attacks-and-availability-attacks.md) | Senior | ML Security |
| 018 | [Detect poisoned documents, anomalous embeddings, reward manipulation, and backdoored adapters.](questions/018-detect-poisoned-documents-anomalous-embeddings-reward-manipulation-and-backdoored-adapters.md) | Principal | Security |
| 019 | [Compare membership inference, model inversion, training-data extraction, and model stealing.](questions/019-compare-membership-inference-model-inversion-training-data-extraction-and-model-stealing.md) | Senior | Privacy |
| 020 | [Apply data minimization, redaction, tokenization, pseudonymization, differential privacy, and retention.](questions/020-apply-data-minimization-redaction-tokenization-pseudonymization-differential-privacy-and-r.md) | Principal | Privacy |
| 021 | [Prevent cross-tenant leakage through retrieval, KV/prompt caches, semantic caches, adapters, and telemetry.](questions/021-prevent-cross-tenant-leakage-through-retrieval-kv-prompt-caches-semantic-caches-adapters-a.md) | Principal | Security |
| 022 | [Secure model artifacts with hashes, signatures, SBOMs, provenance attestations, scanning, and isolated loading.](questions/022-secure-model-artifacts-with-hashes-signatures-sboms-provenance-attestations-scanning-and-i.md) | Principal | Supply Chain |
| 023 | [Evaluate memorization and verbatim regurgitation without exposing sensitive ground truth.](questions/023-evaluate-memorization-and-verbatim-regurgitation-without-exposing-sensitive-ground-truth.md) | Principal | Privacy Evaluation |

## Agent and runtime security

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 024 | [Prevent confused-deputy attacks, capability escalation, stale approval, and credential misuse.](questions/024-prevent-confused-deputy-attacks-capability-escalation-stale-approval-and-credential-misuse.md) | Principal | Agent Security |
| 025 | [Sandbox code, browser, file, network, package, and subprocess access.](questions/025-sandbox-code-browser-file-network-package-and-subprocess-access.md) | Principal | Runtime Security |
| 026 | [Validate generated SQL, shell, HTTP, email, payment, and infrastructure actions.](questions/026-validate-generated-sql-shell-http-email-payment-and-infrastructure-actions.md) | Principal | Security |
| 027 | [Design secrets brokerage, short-lived credentials, scoped capabilities, and emergency revocation.](questions/027-design-secrets-brokerage-short-lived-credentials-scoped-capabilities-and-emergency-revocat.md) | Principal | Security Architecture |
| 028 | [Secure MCP/tool/plugin registries against typosquatting, schema substitution, and malicious updates.](questions/028-secure-mcp-tool-plugin-registries-against-typosquatting-schema-substitution-and-malicious.md) | Principal | Supply Chain |

## Safety, governance, and end-to-end design

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 029 | [Compare rule engines, moderation classifiers, LLM policy judges, refusal training, and human escalation.](questions/029-compare-rule-engines-moderation-classifiers-llm-policy-judges-refusal-training-and-human-e.md) | Senior | Safety |
| 030 | [Balance safety false positives, false negatives, disparate impact, and useful safe completion.](questions/030-balance-safety-false-positives-false-negatives-disparate-impact-and-useful-safe-completion.md) | Principal | Safety |
| 031 | [Design red-team programs, abuse-case taxonomies, continuous adversarial testing, and release gates.](questions/031-design-red-team-programs-abuse-case-taxonomies-continuous-adversarial-testing-and-release.md) | Principal | Safety Engineering |
| 032 | [Build safety incident detection, triage, containment, evidence preservation, and postmortem processes.](questions/032-build-safety-incident-detection-triage-containment-evidence-preservation-and-postmortem-pr.md) | Principal | Operations |
| 033 | [Translate governance into data/model cards, lineage, approvals, audit logs, ownership, and change control.](questions/033-translate-governance-into-data-model-cards-lineage-approvals-audit-logs-ownership-and-chan.md) | Principal | Governance |
| 034 | [Evaluate fairness across demographic groups, languages, dialects, geography, disability, and intersectional slices.](questions/034-evaluate-fairness-across-demographic-groups-languages-dialects-geography-disability-and-in.md) | Principal | Evaluation |
| 035 | [Design a secure enterprise agent with retrieval, side effects, approval, audit, and graceful degradation.](questions/035-design-a-secure-enterprise-agent-with-retrieval-side-effects-approval-audit-and-graceful-d.md) | Principal | System Design |
| 036 | [Diagnose a system that passes benign safety tests but fails under retrieved or multimodal adversarial content.](questions/036-diagnose-a-system-that-passes-benign-safety-tests-but-fails-under-retrieved-or-multimodal.md) | Principal | Debugging |

## Standards and research anchors

- [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/llm-top-10/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [MITRE ATLAS](https://atlas.mitre.org/)
- [SLSA supply-chain security framework](https://slsa.dev/)
