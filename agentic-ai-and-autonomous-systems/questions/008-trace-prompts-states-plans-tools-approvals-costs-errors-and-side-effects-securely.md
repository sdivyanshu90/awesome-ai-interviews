### Q: Trace prompts, states, plans, tools, approvals, costs, errors, and side effects securely.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Capture a trace spanning model version/settings, rendered prompts, state transitions, tool schemas/arguments/results, retrieval provenance, policy decisions, latency, cost, and approvals—redacted and access-controlled. Replay uses recorded inputs or deterministic fakes for side effects.
* **The Deep Dive:** A trace ID links nested calls. State snapshots make divergence inspectable; spans identify bottlenecks. Record prompt/template and tool versions because identical prose under a different schema or model is not comparable. Replay modes must not repeat writes; use sandboxed tools or recorded observations.
A trace is a causal event log: run/step IDs, parent edge, state version/hash, prompt/template/model, decision, validated arguments, tool attempt/result hash, approval, latency, tokens, cost, and side-effect receipt. Store sensitive payloads separately under access controls and keep redacted searchable metadata. Replay has two modes: exact playback from recorded observations and re-execution against fixtures. Live re-execution is not deterministic and must never repeat side effects without idempotency safeguards.

* **Production Reality & Tradeoffs:** Logging prompts can collect sensitive data, so apply minimization, redaction, encryption, retention, and role-based access. Sampling logs blindly may miss rare safety failures.
* **Red Flag:** Logging only final answers and claiming the system is debuggable.
