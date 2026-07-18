### Q: Explain LLM05 Improper Output Handling leading to SQL, shell, HTML, SSRF, or code injection.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Model output is untrusted input to downstream interpreters. Parse into typed structures, validate semantics/authorization, parameterize commands, encode for sinks, and sandbox execution.
* **The Deep Dive:** SQL uses prepared statements and allowlisted query ASTs; shell avoids string execution and passes fixed argv; HTML is context-encoded/sanitized; URLs enforce scheme/host/IP/redirect/egress policy to prevent SSRF; generated code runs in isolated resource-limited sandboxes. Tool schemas only establish syntax—business checks remain.
* **Production Reality & Tradeoffs:** Validation must occur at the sink because transformations change context. Repair loops never execute partial output. Monitor attempted violations.
Model output is attacker-influenced text, never executable authority. Parse into a typed AST/IR, reject unknown fields, validate semantics against current state, parameterize SQL, allowlist HTTP hosts/methods, escape output by sink, and execute in least-privilege sandboxes. For shell, prefer a structured operation API over command strings. SSRF defenses resolve DNS/IP and block metadata/private ranges at execution time, including redirects.

* **Red Flag:** Trusting generated output because the prompt requested safe code.
