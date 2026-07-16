### Q: Diagnose why a playground prompt fails under production serialization, context, traffic, or model versions.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Diff the exact serialized request and complete execution bundle, then replay production cases stage by stage. Most gaps are hidden defaults, role/template/tokenization, truncation, tools/retrieval, concurrency/cache, provider version, or population shift—not prompt wording alone.
* **The Deep Dive:** Capture raw input, rendered roles/messages, byte/token IDs, truncation and context order, retrieved chunks, memory, tool schemas/results, model/provider/version, decoding, safety policy, cache outcome, and response parser. Reproduce with the same request ID/seed where possible. Compare playground and production as canonical JSON and token sequence. Common causes: playground implicit system prompt or latest model; different chat template/special tokens; escaped braces/newlines; dynamic timestamps; prompt cut by token budgeting; production RAG distractors/injection; tool names/schema collisions; streaming parser behavior; safety filters; stale semantic/prefix cache; batching nondeterminism; retries; and real traffic languages/lengths.

Bisect by replacing production stages with recorded playground equivalents: serialization, context, model, decoding, postprocessing. Analyze failures by slice and dependency, not one anecdote. Add the minimized case and transformations to regression tests.
* **Production Reality & Tradeoffs:** Closed APIs may not support exact deterministic replay; use paired distributions. Logging full prompts risks privacy, so store redacted/encrypted controlled artifacts or hashes plus replay fixtures. Fix the causal stage before prompt patching.
* **Red Flag:** Copying the visible playground text into production and assuming the requests are identical.

