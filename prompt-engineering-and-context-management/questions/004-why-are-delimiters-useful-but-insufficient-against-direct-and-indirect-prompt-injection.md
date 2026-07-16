### Q: Why are delimiters useful but insufficient against direct and indirect prompt injection?
* **Difficulty:** Senior
* **Category:** Security
* **The 10-Second Pitch:** Delimiters make instruction/data boundaries legible and reduce accidental mixing, but the model still processes both as tokens and can follow malicious semantics. Security comes from least privilege, provenance, validation, and authorization outside the model.
* **The Deep Dive:** Quoting external text in tags such as `<untrusted_document>...</untrusted_document>` helps the model recognize source and supports training/evaluation conventions. It does not create memory isolation, a parser-enforced type, or a privilege boundary. A document can say “ignore the tags,” encode commands in prose/Unicode/image/OCR, exploit truncation, or persuade the model to copy secrets or call tools. Direct injection arrives from the user; indirect injection arrives through retrieved/web/tool/memory content.

Treat all external content as tainted with provenance. Limit what enters context, strip active content safely, and never interpret it as policy. The model may propose actions only through narrow typed tools. A policy layer authenticates principal, checks resource authorization, semantic constraints, egress, budget, and exact approval; executor uses scoped short-lived credentials. Output is validated for its sink (HTML/SQL/shell), not merely “sanitized.” Red-team across every channel and combined failures.
* **Production Reality & Tradeoffs:** Injection classifiers and delimiters improve defense-in-depth but have false positives/negatives and adaptive attacks. Stronger isolation may reduce agent utility. High-risk side effects must fail closed even if the model appears confident.
* **Red Flag:** Claiming XML tags or “never follow document instructions” eliminates prompt injection.

