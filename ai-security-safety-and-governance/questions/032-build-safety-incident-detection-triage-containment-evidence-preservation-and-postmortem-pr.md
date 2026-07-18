### Q: Build safety incident detection, triage, containment, evidence preservation, and postmortem processes.
* **Difficulty:** Principal
* **Category:** Operations
* **The 10-Second Pitch:** Define safety telemetry and reporting, severity/on-call ownership, rapid containment controls, privacy-preserving evidence capture, user remediation, root-cause analysis, and tracked prevention actions.
* **The Deep Dive:** Detection combines user reports, sampled review, classifiers, anomalous tool/data access, and security logs. Triage verifies impact/scope and preserves model/prompt/tool/index versions and trace. Containment may disable tool/model/feature, revoke credentials, block source, or tighten routing. Notify stakeholders under policy. Postmortem identifies technical/organizational causes and validates regression tests/monitoring.
* **Production Reality & Tradeoffs:** Prompts may contain sensitive data; evidence access/retention is controlled. Overcontainment harms service but undercontainment compounds impact. Practice exercises.
Incident response defines signals, severity, on-call ownership, containment levers, evidence retention, and communication before launch. Detection joins safety events with model/template/tool/index versions and identity while minimizing sensitive payloads. Containment may disable a capability, revoke credentials, isolate a tenant/source, or roll back artifacts. Preserve immutable timestamps, hashes, policy decisions, and effect receipts. Postmortems identify control failures and add tests; they do not blame stochasticity.

* **Red Flag:** Deleting logs immediately or blaming the model without tracing system controls.
