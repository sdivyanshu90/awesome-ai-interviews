### Q: Design a licensed, provenance-tracked, deduplicated, contamination-resistant SFT dataset.
* **Difficulty:** Principal
* **Category:** Data
* **The 10-Second Pitch:** Define the target behavior and evaluation first; collect licensed, provenance-tracked examples; deduplicate and decontaminate; label consistently; split by entity/time/template; and include hard counterexamples, refusals, and representative failures.
* **The Deep Dive:** Normalize templates but retain raw source. Audit labeler agreement and distinguish factual correctness, policy compliance, style, and tool behavior. Near-duplicate examples across train/test inflate results; template-only splits miss semantic leakage. Synthetic data needs independent verification because a teacher can transfer its systematic errors.
Build data from a behavior taxonomy: desired action, required evidence, forbidden action, ambiguity, and recovery. Preserve license/source IDs, cluster semantic duplicates, then split by source, entity, template, and time. Track token mass because long sources can dominate balanced row counts. Synthetic examples need independent verification. Reject unresolved PII, consent gaps, weak provenance, inconsistent labels, and benchmark contamination.

* **Production Reality & Tradeoffs:** Dataset versioning, deletion handling, PII policy, and licensing are release blockers. More examples of one easy pattern can make a model look better while narrowing generalization.
* **Red Flag:** Randomly splitting rows after templating the same source document into many near-identical examples.
