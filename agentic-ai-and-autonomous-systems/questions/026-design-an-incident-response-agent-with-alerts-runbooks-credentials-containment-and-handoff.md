### Q: Design an incident-response agent with alerts, runbooks, credentials, containment, and handoff.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Use a durable, least-privilege state machine: correlate alerts, gather read-only evidence, select versioned runbooks, propose containment, require risk-based approval, execute idempotently, verify recovery, and hand off an auditable timeline.
* **The Deep Dive:** Ingestion deduplicates alerts into incidents with severity and affected resources. Read tools query logs/metrics/config snapshots. The model summarizes hypotheses with evidence; deterministic runbook preconditions and policy determine permitted actions. Credentials are short-lived and scoped per action. Destructive isolation/failover needs approval or pre-authorized emergency policy. After action, health checks verify effect and rollback/compensation. Human handoff includes evidence, actions, unknowns, and next decisions.

  Containment requires stronger authority than diagnosis: safe reversible probes may run automatically, while credential revocation, traffic blocking, host isolation, or rollback demands currently scoped credentials and usually approval. Preserve timestamps, commands, evidence hashes, and effect receipts, and escalate rather than improvise when telemetry is incomplete or the runbook conflicts with observed state.

* **Production Reality & Tradeoffs:** Automation can amplify outages. Separate diagnosis from action, rehearse in simulations, provide kill switch, and never let unavailable observability become evidence of recovery. Protect incident data/secrets.
* **Red Flag:** Giving the model administrator credentials because response must be fast.
