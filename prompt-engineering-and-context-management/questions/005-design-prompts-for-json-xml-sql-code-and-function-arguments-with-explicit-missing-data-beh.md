### Q: Design prompts for JSON, XML, SQL, code, and function arguments with explicit missing-data behavior.
* **Difficulty:** Mid
* **Category:** Implementation
* **The 10-Second Pitch:** Define a machine-readable schema and missing/conflict policy, use constrained decoding where available, parse strictly, validate syntax and semantics, and retry only bounded repair. Treat generated SQL/code/actions as untrusted proposals.
* **The Deep Dive:** Specify exact fields, types, enums, cardinality, units, nullability, and whether unknown is `null`, omitted, or an explicit status—never tell the model to guess. Give one minimal valid example plus edge cases for missing/conflicting evidence. For JSON/function calls, grammar/schema-constrained decoding prevents syntax outside the language; still run a strict parser, JSON Schema/type validator, normalization, cross-field/business rules, size limits, and provenance checks. XML additionally needs entity/DTD disabled and namespace rules.

SQL should target a typed query plan or allowlisted AST: parse, restrict statements/tables/functions, parameterize literals, enforce row/tenant scope, cost/row limits, and execute read-only in a sandbox. Code requires path/API constraints, static analysis, isolated tests, and review. Function arguments need independent authorization and approval.

```text
prompt + schema -> constrained generation -> strict parser -> semantic/policy validation
                                  failure -> bounded repair or abstain
```
* **Production Reality & Tradeoffs:** Constrained decoding adds latency and cannot guarantee truthful values. Repair loops can amplify cost or change intent; cap attempts and preserve errors. Schema versions are APIs and need compatibility/canaries.
* **Red Flag:** Using “output valid JSON only” as the validator, or executing generated SQL/tool arguments after successful parsing alone.

