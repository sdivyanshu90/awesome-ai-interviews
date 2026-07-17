### Q: Implement a durable tool-calling state machine with budgets, idempotency, approval, and replay.
* **Difficulty:** Mid
* **Category:** Coding
* **The 10-Second Pitch:** Represent execution as typed states/events; validate every transition; reserve budget; persist before/after side effects; bind approvals to action digests; use idempotency keys; and replay with recorded tool observations.
* **The Deep Dive:** State contains goal, step, tool proposal, authorization result, budget ledger, approvals, completed effects, and terminal status. Node returns `next_state` plus events. Before a write, canonicalize arguments, hash action, persist pending record, check approval, then call with idempotency key. On timeout, query operation status before retry. Checkpoint transactionally. Replay mode injects stored results and disables executors; migration tests old state versions. Unit tests cover crashes at every boundary, duplicate delivery, expired approval, budget exhaustion, and compensation.
* **Production Reality & Tradeoffs:** Exactly-once effects usually require cooperation from the downstream service; otherwise design at-least-once plus idempotency/compensation. Durable logs contain sensitive data and need controls.
Implement the loop as compare-and-swap transitions on a versioned run record. Each tool intent is persisted before dispatch with a stable idempotency key; its result is committed once. Approval records bind action hash, actor, expiry, and state version. Recovery scans intents without terminal results and queries operation status before retrying. Deterministic reducers and explicit error states make replay possible; a while-loop around model text does not.

* **Red Flag:** Implementing the workflow as an in-memory while loop around chat history.
