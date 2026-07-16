### Q: How should untrusted documents, webpages, emails, and tool outputs be represented in context?
* **Difficulty:** Senior
* **Category:** Security
* **The 10-Second Pitch:** Represent them as typed, provenance-rich evidence with explicit trust labels and inert serialization; never as instructions. Minimize fields and preserve origin/ACL/time, while deterministic policy controls what the model may do with them.
* **The Deep Dive:** Normalize/sanitize active formats in a sandbox; extract visible text, metadata, OCR, links, and attachments while disabling scripts/macros/entities. Wrap each item with source ID, author/connector, fetch/event time, tenant/ACL, content type, version/hash, and `trust=untrusted`. Quote content using length-delimited/escaped encoding that cannot close its container; however delimiters are cues, not security boundaries. Tell the model evidence may contain malicious instructions and require citations.

Tool output is also untrusted data: schema-validate, cap size, redact secrets, and separate status/typed fields from free text. External content cannot change policy, tools, permissions, memory, or approval. Model action proposals go through deterministic schema, authorization, business, egress, budget, and approval checks. Preserve provenance through summaries/citations and quarantine detected payloads.

```text
external bytes -> sandbox parse -> typed provenance envelope -> context as evidence
model proposal -> independent policy/authorization -> executor
```
* **Production Reality & Tradeoffs:** Over-sanitization removes useful layout/code; under-sanitization carries active content. Injection detection is imperfect. Limit retrieval and tool scopes so model compromise has low blast radius.
* **Red Flag:** Putting web/tool text after “SYSTEM:” or trusting it because a server fetched it.

