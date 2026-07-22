### Q: Resolve conflicting agent outputs using verification, provenance, voting, arbitration, or policy.
* **Difficulty:** Principal
* **Category:** Multi-Agent Systems
* **The 10-Second Pitch:** Resolve by the strongest available ground truth: deterministic tests, source provenance, policy constraints, calibrated expert arbitration, then voting only when errors are sufficiently independent. Preserve disagreement instead of forcing false consensus.
* **The Deep Dive:** Normalize outputs into claims/actions with evidence. Reject policy-invalid candidates first. Execute tests or queries for falsifiable claims. Rank source authority, recency, and directness; check whether agents relied on the same source. Weighted voting requires calibrated historical reliability and independence assumptions. An arbiter receives blinded candidates and evidence, applies an explicit rubric, and may request new information or escalate. For irreversible actions, disagreement raises the approval threshold.

  Reserve voting for low-risk decisions; confidence-weighted voting without calibration is theater. Policy constraints override any consensus, and “no supported answer” remains a valid terminal result—forcing a merge of contradictory outputs is itself a failure. Counterexample for naive voting: five agents that all read the same poisoned document outvote the one agent that queried the live system, so provenance-aware resolution must run before any votes are counted.

* **Production Reality & Tradeoffs:** Arbiters can share model bias or be prompt-injected. Log resolution reasons and evaluate conflict cases separately. Consensus is not correctness.
* **Red Flag:** Choosing the most confident or verbose agent.
