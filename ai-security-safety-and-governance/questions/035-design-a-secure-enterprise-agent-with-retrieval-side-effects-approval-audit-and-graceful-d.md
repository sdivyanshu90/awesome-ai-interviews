### Q: Design a secure enterprise agent with retrieval, side effects, approval, audit, and graceful degradation.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Authenticate identity, retrieve only authorized provenance-rich evidence, expose least-privilege tools, validate/approve immutable actions, execute idempotently with scoped credentials, and preserve an auditable state machine with safe fallbacks.
* **The Deep Dive:** Authenticate the human and organization first; propagate principal, tenant, purpose, and delegated authority as structured metadata rather than prompt text. Retrieval performs ACL filtering before approximate search and reranking, attaches source/version/digest, and rejects content whose policy classification is incompatible with the task. The model receives untrusted evidence and can produce either a cited answer or a typed action proposal. A policy enforcement point canonicalizes arguments and independently checks schema, ownership, row/resource scope, business rules, egress, spend, and rate limits. Risky actions produce an exact diff/preview and approval envelope bound to principal, tool, canonical arguments, resource version, and expiry.

```text
identity -> risk router -> ACL retrieval -> untrusted evidence -> model proposal
   |                                                        |
   +--------------------------------------------------------v
                              deterministic policy + authorization
                                             |
                              low risk -------+------- high risk
                                  |                      |
                                  |                bound approval
                                  +----------+-----------+
                                             v
                              JIT credential -> idempotent executor
                                             |
                                 verified result + append-only audit
```

  The executor receives a single-use or short-lived capability, not the user’s standing credential. It uses an idempotency key, explicit prepare/commit boundary, timeout, and result verification. Conversation state records proposed versus approved versus committed operations so retries cannot duplicate effects. The audit event includes model/prompt/policy/index/tool versions, evidence IDs, authorization decision, approval digest, and outcome without unnecessarily copying sensitive prompt content. Degraded modes are explicit: read-only answer, cached verified information, queue for human handling, or abstention.
* **Production Reality & Tradeoffs:** Scope retrieval, memory, adapters, prompt/KV caches, and telemetry by tenant and policy domain. Approval and audit add latency; route by action risk and preserve a fast path for reversible reads. Design for policy/model/tool outages, partial commits, approval expiry, deletion, and region residency. Operational controls require capability revocation, credential rotation, index quarantine, replay-safe recovery, and a tested kill switch. Final-output filtering cannot undo an unauthorized side effect.
* **Red Flag:** Giving the agent broad access then filtering its final response.
