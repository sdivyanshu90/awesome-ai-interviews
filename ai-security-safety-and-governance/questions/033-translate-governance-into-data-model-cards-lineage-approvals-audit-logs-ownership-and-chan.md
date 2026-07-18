### Q: Translate governance into data/model cards, lineage, approvals, audit logs, ownership, and change control.
* **Difficulty:** Principal
* **Category:** Governance
* **The 10-Second Pitch:** Governance becomes executable when every artifact has provenance, risk classification, accountable owner, evaluation evidence, approval state, deployment constraints, and auditable changes/rollback.
* **The Deep Dive:** Data cards document source, consent/license, composition, preprocessing, sensitive attributes, limitations, and deletion. Model cards record objective, data lineage, metrics/slices, safety, intended/prohibited use, compute, and versions. Registry links model-prompt-index-tool configs to approvals. Change control defines materiality, tests, reviewers, canary, rollback, and retention. Audit logs record decisions/effects with access control.
* **Production Reality & Tradeoffs:** Documents drift unless generated from pipelines and enforced at release. Governance requirements vary by jurisdiction/use case; legal interpretation needs qualified owners.
Operational governance maps each obligation to an owner, artifact, control, evidence, review cadence, and exception expiry. Data cards capture source, license, consent, transformations, and limits; model cards capture intended use and evaluation; lineage links both to deployed versions. Change control requires risk-tiered approval and rollback. Audit logs must be access-controlled, tamper-evident, retention-limited, and reconstruct decisions without storing unnecessary prompt content.

* **Red Flag:** Producing a model card after deployment with no operational gate.
