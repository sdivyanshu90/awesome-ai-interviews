### Q: How should prompts, examples, schemas, tool definitions, and decoding settings be versioned together?
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Treat the full request contract as one immutable bundle: rendered templates, demonstrations, schemas/tools, model/provider, tokenizer, decoding, retrieval/policy versions, and compatibility. Evaluate and deploy the bundle, never a prompt string alone.
* **The Deep Dive:** Store source template plus deterministic renderer, role ordering, examples with provenance, response schema, tool names/descriptions/JSON schemas, safety/policy snippets, model alias resolved to version, tokenizer/chat template, temperature/top-p/max tokens/seed, and context-builder/retrieval configuration. Compute a content digest and emit it in every trace. Type-check variables and escape untrusted values by context; snapshot the exact serialized request for replay.

Promotion pipeline runs golden/adversarial/metamorphic/replay suites with paired uncertainty, schema/tool/safety gates, token/latency/cost diffs, then shadow and canary. Compatibility tests cover provider role/tool semantics and schema evolution. Rollback points routing to a previously warm immutable bundle; cache keys include bundle identity and stateful index/schema migrations remain backward-compatible.

```text
source bundle -> deterministic render -> evaluation -> shadow -> canary -> production
       ^                                                       |
       +---------------- immutable rollback -------------------+
```
* **Production Reality & Tradeoffs:** Provider behavior can change behind mutable aliases, so archive responses/metadata and monitor drift. More bundle fields reduce accidental changes but increase governance. Secrets belong in brokers, never versioned prompts.
* **Red Flag:** Versioning only the system string while examples, tool schemas, model alias, and sampling change independently.

