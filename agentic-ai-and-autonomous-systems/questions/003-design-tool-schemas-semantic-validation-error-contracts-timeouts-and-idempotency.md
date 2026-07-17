### Q: Design tool schemas, semantic validation, error contracts, timeouts, and idempotency.
* **Difficulty:** Senior
* **Category:** Systems
* **The 10-Second Pitch:** Tool schemas define typed inputs, but the executor validates semantic constraints, authorization, and idempotency. Every side-effecting call needs an idempotency key, bounded retry policy, timeout, audit record, and a way to distinguish unknown completion from failure.
* **The Deep Dive:** JSON schema checks syntax/type; business validation checks account scope, amount limits, state transitions, and referential integrity. Retries after timeouts can duplicate an action unless server-side idempotency records the key/result. Tool responses should return structured status, evidence, and safe error codes rather than raw stack traces.
A schema proves syntax, not semantic validity. The executor checks identity, resource ownership, currency/unit ranges, stale versions, and invariants, then binds an idempotency key to canonical arguments and operation type. Retries return the recorded result instead of repeating the effect. Distinguish timeout-before-commit from timeout-after-commit using operation status or a transactional outbox. Model-facing errors are typed and minimal; raw stack traces and secrets stay out of context.

* **Production Reality & Tradeoffs:** Separate read and write tools; require confirmation for irreversible actions. Model retries are not transport retries—centralize retry behavior in the executor.
* **Red Flag:** Assuming schema-valid tool arguments are authorized and safe to execute.
