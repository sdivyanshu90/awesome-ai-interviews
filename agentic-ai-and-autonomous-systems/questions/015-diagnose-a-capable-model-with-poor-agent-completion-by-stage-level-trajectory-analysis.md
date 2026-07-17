### Q: Diagnose a capable model with poor agent completion by stage-level trajectory analysis.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Agent success is limited by the weakest interface: ambiguous goal parsing, missing state, bad retrieval, brittle tool schema, authorization failures, stale observations, poor stop rules, or untested recovery can dominate model reasoning. Diagnose traces by failure stage and counterfactual replay.
* **The Deep Dive:** Build a funnel: task understood → plan valid → tool selected → arguments valid → tool succeeds → result interpreted → policy compliant → task verified. Compare model-only simulation with real tools to separate reasoning from integration. Check whether the environment provides enough observability for the next action.
Decompose completion into perception/retrieval, planning, argument generation, tool availability, execution, observation interpretation, state update, and stopping. Build a funnel with conditional success at each stage and inspect failure transitions. A stronger model may still suffer from ambiguous schemas, truncated observations, stale state, missing retries, or impossible permissions. Run controlled ablations—gold tool choice, gold arguments, gold observations—to locate the bottleneck before changing the base model.

* **Production Reality & Tradeoffs:** Improve deterministic scaffolding before upgrading models. The most valuable change is often a new read-only inspection tool, typed state field, or clearer tool error—not a longer prompt.
* **Red Flag:** Replacing the model immediately without inspecting trajectories and tool outcomes.
