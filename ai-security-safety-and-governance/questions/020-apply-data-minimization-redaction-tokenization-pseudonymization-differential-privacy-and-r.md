### Q: Apply data minimization, redaction, tokenization, pseudonymization, differential privacy, and retention.
* **Difficulty:** Principal
* **Category:** Privacy
* **The 10-Second Pitch:** Minimize collection first; redact unnecessary fields; tokenize/pseudonymize identifiers when linkage is required; use DP for bounded statistical/training privacy; retain only for defined purpose and duration.
* **The Deep Dive:** Classify fields and purpose. Redaction removes/masks content; tokenization replaces with vault-backed surrogate; pseudonymization remains re-identifiable with auxiliary data. DP clips contribution and adds calibrated noise, tracking `(ε,δ)` composition; it is not generic encryption. Retention applies to prompts, outputs, logs, datasets, embeddings, checkpoints, caches, and backups. Access/deletion are auditable.
* **Production Reality & Tradeoffs:** Over-redaction harms task accuracy; under-redaction leaks. DP at LLM scale is costly and group privacy/rare text remain challenging. Verify transformations, not policy documents only.
Minimization removes fields and examples not required for purpose. Redaction deletes sensitive spans; tokenization replaces values with vault-backed tokens; pseudonymization keeps linkability under a separate key; differential privacy bounds one record’s influence with $(\epsilon,\delta)$ under stated accounting; retention limits time. These are not interchangeable. Apply before logging/training when possible, manage re-identification through quasi-identifiers, and propagate deletion across datasets, embeddings, caches, traces, and artifacts.

* **Red Flag:** Calling hashed identifiers anonymous regardless of entropy/linkage.
