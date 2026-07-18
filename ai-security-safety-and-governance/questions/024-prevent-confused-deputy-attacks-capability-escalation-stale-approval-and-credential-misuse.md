### Q: Prevent confused-deputy attacks, capability escalation, stale approval, and credential misuse.
* **Difficulty:** Principal
* **Category:** Agent Security
* **The 10-Second Pitch:** Bind actions to the initiating principal, exact resource, capability, and current policy. Use scoped short-lived credentials and approvals tied to immutable action digests; reauthorize on change.
* **The Deep Dive:** The agent cannot borrow orchestrator privilege. Capability tokens encode tenant/user/action/resource/limits/expiry and are checked by the tool. Approval preview includes arguments/evidence; hash and persist it. Any replan/argument change invalidates approval. Secret broker issues just-in-time credentials to executor, never model context. Audit identity chain and revoke rapidly.
* **Production Reality & Tradeoffs:** Long workflows face identity/policy changes; revalidate before write. Approval fatigue is dangerous. Design safe read-only fallbacks.
A confused deputy has legitimate privilege but acts for an unauthorized requester. Bind every capability to subject, tenant, resource, operation, argument constraints, purpose, expiry, and state version. The executor verifies these claims at use time; delegation can only narrow them. Approvals bind the exact canonical action hash and expire after state changes. Short-lived credentials reduce blast radius, while idempotency and receipts prevent retry-based duplicate effects.

* **Red Flag:** Using one powerful service account for all users/tools.
