### Q: Explain LLM02 Sensitive Information Disclosure across prompts, outputs, caches, logs, and training data.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Sensitive data leaks when it is unnecessarily placed in context, memorized, retrieved across scope, cached/logged broadly, or emitted through output/tool channels. Minimize before the model and control every derivative.
* **The Deep Dive:** Classify data, redact/tokenize where possible, enforce retrieval ACLs, isolate tenant memory/cache, and broker secrets executor-side. Outputs pass DLP/policy where justified and render safely. Logs store metadata/minimized payload with encryption, retention, and RBAC. Training requires consent/provenance, dedup, PII controls, and extraction tests. Deletion propagates to derivatives.
* **Production Reality & Tradeoffs:** DLP has false positives/negatives and encrypted storage does not prevent authorized-process leakage. The safest secret is one never exposed to the model.
Disclosure surfaces include prompt assembly, model output, retrieval snippets, embeddings, semantic/prefix/KV caches, traces, feedback exports, training data, and support tooling. Minimize before ingestion; scope access before retrieval; redact before logging; isolate caches by tenant/model/template/policy; set retention and deletion. System prompts are not secret vaults. Canary secrets detect leakage, but real credentials should never enter model context when a brokered capability suffices.

* **Red Flag:** Putting API keys in a system prompt because users cannot normally see it.
