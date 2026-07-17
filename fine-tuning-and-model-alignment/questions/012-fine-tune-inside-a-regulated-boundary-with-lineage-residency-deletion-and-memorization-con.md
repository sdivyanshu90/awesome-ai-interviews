### Q: Fine-tune inside a regulated boundary with lineage, residency, deletion, and memorization controls.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Keep governed data in an approved boundary, establish lineage/consent/retention controls before training, minimize and de-identify inputs, use isolated compute and access, validate outputs for memorization, and define a retraining/unlearning response before release.
* **The Deep Dive:** Record dataset version, legal basis, transformations, annotators, model/adapter lineage, and evaluation evidence. Deletion requests propagate through source, derived datasets, checkpoints, adapters, caches, and logs according to policy; exact unlearning may require retraining from a clean checkpoint. External APIs and telemetry are data transfers.
Keep raw regulated records inside the governed region and feed only approved, minimized examples through a lineage ledger. A checkpoint binds dataset snapshot, exclusions, code, base hash, and evaluator versions. Deletion requires stopping sampling, invalidating derivatives, tracing affected artifacts, and applying the promised retraining/unlearning policy. Use access-controlled canaries for memorization tests; export only signed artifacts passing privacy, residency, retention, security, and human gates.

* **Production Reality & Tradeoffs:** Legal permission is not technical safety. Use retrieval with access control when facts must remain current or deletable rather than embedding them in weights.
* **Red Flag:** Claiming that deleting a row from the source table removes its influence from an already trained model.
