### Q: Evaluate task success, trajectory validity, tool correctness, recovery, safety, latency, and cost.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Measure task success, constraint compliance, tool-argument correctness, trajectory efficiency, recovery after failure, safety violations, human-intervention rate, latency, and cost. Final text can look good even when the agent took unsafe or unnecessary actions.
* **The Deep Dive:** Build deterministic simulated tasks with known state transitions, plus replay of anonymized real failures and adversarial tool observations. Score trajectories against allowed actions and evidence; evaluate partial credit and abstention. Run repeated trials because sampling variance and environment nondeterminism change outcomes.
Evaluate the entire trajectory as a structured object. Metrics include completion, valid-action rate, unnecessary steps, precondition violations, recovery after injected faults, authorization failures, evidence provenance, irreversible-effect correctness, latency, tokens, and dollars. Counterfactual replay asks whether a different observation should change the next action. Report success conditional on budget and environment difficulty; final-answer grading alone rewards agents that guess correctly after unsafe or wasteful behavior.

* **Production Reality & Tradeoffs:** A benchmark with only happy paths incentivizes brittle demos. Separate offline capability gates from online monitoring and human review of high-impact tasks.
* **Red Flag:** Evaluating an agent only with an LLM judge reading its final response.
