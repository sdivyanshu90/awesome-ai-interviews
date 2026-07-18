### Q: Run online experiments with interference, novelty, delayed outcomes, guardrails, and heterogeneous effects.
* **Difficulty:** Principal
* **Category:** Online Experimentation
* **The 10-Second Pitch:** Randomize at the unit that prevents spillover, predefine primary/guardrail metrics and duration, model delayed outcomes and novelty, and inspect treatment effects across important populations before rollout.
* **The Deep Dive:** Choose randomization unit—request, user, account, team, region—based on interference and product memory. Use sticky assignment and intent-to-treat analysis. Specify hypothesis, primary metric, minimum effect, sample size, guardrails, stopping rule, and exclusion policy before launch. Network/shared-agent interference may require cluster randomization or switchback designs. Run long enough for weekday cycles and novelty/learning effects; estimate time-varying treatment. Delayed conversions need fixed windows, survival analysis, or matured cohorts. CUPED/covariates reduce variance when pre-treatment. Monitor safety, latency, cost, and complaint guardrails with valid sequential methods. Report heterogeneous effects with multiplicity/shrinkage and avoid post-hoc subgroup stories.
* **Production Reality & Tradeoffs:** Online tests expose users and can miss rare severe harms. Provider/model changes during a test violate stability. Statistical significance is not business or safety significance; retain rollback and ramp gradually.
* **Red Flag:** Randomizing messages when users share memory and then treating observations as independent.

