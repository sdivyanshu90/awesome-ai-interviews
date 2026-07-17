### Q: Resolve conflicting agent outputs using verification, provenance, voting, arbitration, or policy.
* **Difficulty:** Principal
* **Category:** Multi-Agent Systems
* **The 10-Second Pitch:** Resolve by the strongest available ground truth: deterministic tests, source provenance, policy constraints, calibrated expert arbitration, then voting only when errors are sufficiently independent. Preserve disagreement instead of forcing false consensus.
* **The Deep Dive:** Normalize outputs into claims/actions with evidence. Reject policy-invalid candidates first. Execute tests or queries for falsifiable claims. Rank source authority, recency, and directness; check whether agents relied on the same source. Weighted voting requires calibrated historical reliability and independence assumptions. An arbiter receives blinded candidates and evidence, applies an explicit rubric, and may request new information or escalate. For irreversible actions, disagreement raises the approval threshold.
* **Production Reality & Tradeoffs:** Arbiters can share model bias or be prompt-injected. Log resolution reasons and evaluate conflict cases separately. Consensus is not correctness.
Prefer verifiable evidence over status or eloquence. Normalize claims, attach source/tool receipts, and run deterministic checks where possible. Voting is appropriate only when errors are sufficiently independent and the decision is low risk; confidence-weighted voting without calibration is theater. For material conflicts, expose alternatives and evidence to an authorized arbiter or human. Policy constraints override consensus, and “no supported answer” remains a valid result.

* **Red Flag:** Choosing the most confident or verbose agent.
