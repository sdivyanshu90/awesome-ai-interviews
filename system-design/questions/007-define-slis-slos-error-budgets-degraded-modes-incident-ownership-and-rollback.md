### Q: Define SLIs, SLOs, error budgets, degraded modes, incident ownership, and rollback.
* **Difficulty:** Principal
* **Category:** SRE
* **The 10-Second Pitch:** Measure user-visible good events, set achievable percentile/availability objectives by class, spend the error budget deliberately, predefine safe degradation, and attach every alert and rollback to an owner.
* **The Deep Dive:** SLIs should reflect outcomes: successful grounded responses, TTFT/ITL under thresholds, valid tool commits, safety containment, freshness, and task success—not only HTTP 200. Define numerator/denominator, exclusions, windows, regions, and classes. SLOs set targets; error budget is allowed bad events over the window and governs release velocity. Use burn-rate alerts across short/long windows to catch fast and slow exhaustion. Degraded modes may disable tools, use cached verified results, reduce context/model, switch region/provider, queue async, or abstain; each has capability and safety contracts. Runbooks identify incident commander, service/data/model/security owners, evidence, containment, communication, and recovery. Rollback requires immutable compatible bundles and data/index migration strategy.
* **Production Reality & Tradeoffs:** Too many SLOs dilute action; too strict causes alert fatigue and excess cost. Model quality often lacks immediate labels, so combine leading indicators with sampled review and delayed outcomes.
* **Red Flag:** Defining uptime only at the load balancer or having a rollback button never exercised with state/schema changes.

