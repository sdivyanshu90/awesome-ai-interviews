### Q: Design approval gates, previews, capability tokens, and least-privilege tool identities.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Enforce least-privilege, scoped short-lived credentials, policy-engine authorization, typed allowlisted tools, confirmation/dual control for high impact, and immutable audit logs. The model proposes; trusted code authorizes and executes.
* **The Deep Dive:** Capabilities should bind principal, tenant, action, resource, amount, and expiry. Separate read from write permissions and require a user-visible action preview. Policy decisions consider current state and source provenance, not only tool name. Approval interrupts persist the exact proposed action so approval cannot be reused for a changed request.
Separate **proposal** from **authorization**. The model emits a canonical action plan; deterministic policy evaluates actor, resource, operation, parameters, risk, and current version. High-impact actions receive a preview/diff and approval bound cryptographically or transactionally to that exact plan, expiry, and identity. The executor obtains a short-lived capability only after approval, checks it again, and emits a receipt. Editing arguments after approval invalidates the authorization.

* **Production Reality & Tradeoffs:** Emergency revocation and incident kill switches are mandatory. Browser or shell tools need network/file isolation and output sanitization; no prompt can replace sandbox boundaries.
* **Red Flag:** Giving a broadly privileged service account to every agent tool for implementation convenience.
