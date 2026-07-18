### Q: Compare rule engines, moderation classifiers, LLM policy judges, refusal training, and human escalation.
* **Difficulty:** Senior
* **Category:** Safety
* **The 10-Second Pitch:** Rules are deterministic/narrow; classifiers scale known categories; LLM judges handle context but are probabilistic/injectable; refusal training changes base behavior; humans handle ambiguity/high stakes. Layer them by risk.
* **The Deep Dive:** Rules enforce hard product/legal constraints. Classifiers produce calibrated scores with thresholds. Policy LLMs evaluate nuanced intent/output under versioned rubrics but need isolation from untrusted instructions and human validation. Refusal training improves default behavior but may overrefuse and cannot authorize tools. Human reviewers require evidence, privacy, consistency, and appeals. Combine pre-input, generation/tool, and post-output controls.
* **Production Reality & Tradeoffs:** Each layer adds latency and false positives. Measure per-category, language, and benign-boundary utility. Human capacity is finite.
Rule engines are deterministic and auditable for crisp constraints; classifiers are cheap but distribution-sensitive; LLM judges handle nuance but are injectable and inconsistent; refusal training shapes base behavior; humans resolve high-risk ambiguity slowly. Compose them by consequence: deterministic block/authorization at hard boundaries, classifiers for broad triage, model judges for additional evidence, and humans for critical uncertainty. Calibrate thresholds and disagreement routes on representative languages and attack traffic.

* **Red Flag:** Using one moderation endpoint as complete safety architecture.
