### Q: Design defense in depth using instruction/data separation, provenance, least privilege, validation, and approval.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Separate trusted policy from untrusted data, carry source provenance, expose narrow tools, validate proposed actions semantically, and bind human approval to immutable high-risk operations.
* **The Deep Dive:** At ingestion, normalize safely, malware-scan active formats, and attach immutable origin, tenant, ACL, version, and content digest. Treat retrieved/tool/web content as quoted evidence, never as privileged instruction; this semantic separation helps the model but is not a security boundary. Build context from policy-compatible sources and propagate provenance into citations and proposed actions. A capability registry exposes narrow typed operations—prefer `create_draft_invoice` over unrestricted SQL or shell—and scopes resources, fields, amount, network destination, and call count to the authenticated principal.

  The model may only propose. A deterministic enforcement path validates JSON schema, canonicalizes arguments, evaluates business invariants, rechecks current resource authorization, applies egress/DLP policy, and computes risk. High-risk actions render an exact preview and digest over `{principal, tool, canonical_args, resource_version, expiry}`. Approval signs that digest; any argument or state change invalidates it. The executor obtains a just-in-time credential for only that action, uses an idempotency key, verifies the result, and records proposal, policy decision, approval, execution, and response versions. Detection monitors injection indicators, unusual tool sequences, denied calls, volume, and outbound destinations; a kill switch revokes capabilities independently of the model.
Defense-in-depth maps controls to transitions:
```text
untrusted input -> provenance/normalization -> model proposes typed intent
                -> deterministic policy/authorization -> preview/approval
                -> sandboxed executor -> validated result -> audit
```
No classifier is a universal sanitizer. Minimize data/tool exposure, use least-privilege identities, bind approvals to exact arguments, and fail closed on policy uncertainty. Test bypass combinations because individually weak controls often fail compositionally.

* **Production Reality & Tradeoffs:** Assume every layer, including classifier and model, can fail and test combinations such as injected retrieval plus stale approval plus tool timeout. Authorization, tenant isolation, and spend limits fail closed; low-risk conversational functionality may fail open only by explicit policy. Added checks increase latency and false refusals, so tier actions by reversibility, sensitivity, and blast radius. Human approval is not a cure if previews are ambiguous, approvals are reusable, or reviewers are overwhelmed.
* **Red Flag:** Relying on a single prompt-injection classifier.
