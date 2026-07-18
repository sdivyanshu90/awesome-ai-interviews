### Q: Build canary, shadow, A/B, rollback, compatibility, and model/prompt/index migration processes.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Version the full serving bundle, replay/shadow first, canary small traffic with guardrails, run controlled experiments, and retain fast rollback plus dual compatibility for stateful migrations.
* **The Deep Dive:** Bundle model/tokenizer/template/adapter, engine, decoding, tools, safety, embedding/index, and schemas. Offline regression and load test; shadow records outputs without user impact; canary monitors quality/safety/latency/cost by slice. A/B randomizes eligible units. Index/model migrations dual-write/read and pin conversations to compatible versions. Rollback restores bundle and handles new state schema.
* **Production Reality & Tradeoffs:** Shadowing costs capacity and may process sensitive data. Rollback cannot undo side effects or newly written incompatible data; design forward/backward migrations.
A release unit includes model weights, tokenizer, template, runtime/engine, quantization, adapter, safety policy, retrieval index/schema, and tool contracts. Shadow compares outputs without effects; canary limits exposure; A/B estimates user impact with guardrails; rollback restores the whole compatible bundle. Persist cohort assignment and correct interference/novelty. Schema adapters support mixed versions during migration. Define automatic rollback on safety, error, quality, or p99 thresholds with confidence and minimum sample rules.

* **Red Flag:** Canarying only model weights while silently changing tokenizer/prompt.
