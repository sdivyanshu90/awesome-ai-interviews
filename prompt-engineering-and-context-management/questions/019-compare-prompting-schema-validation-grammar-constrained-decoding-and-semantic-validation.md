### Q: Compare prompting, schema validation, grammar-constrained decoding, and semantic validation.
* **Difficulty:** Senior
* **Category:** Implementation
* **The 10-Second Pitch:** Prompting nudges content, grammar decoding guarantees syntactic membership, schema validation checks parsed types/structure, and semantic validation enforces domain truth, authorization, and invariants. Reliability needs all applicable layers.
* **The Deep Dive:** A prompt describes the desired format but sampling can violate it. Grammar-constrained decoding maintains parser/automaton state and masks tokens that cannot extend a valid language, guaranteeing syntactic JSON/regex/CFG when tokenizer-byte handling is correct. A schema validator runs after parsing and checks required fields, types, enums, ranges, and structural constraints. Semantic validation checks facts and operations the schema cannot express: resource belongs to principal, dates are ordered, totals match, source evidence supports values, SQL is read-only, and payment is within policy.

```text
prompt intent -> grammar-constrained tokens -> parse -> schema -> semantic/policy -> accept/repair/reject
```

Missing-data/conflict behavior must be in both contract and schema (`status`, `null`, evidence). Repair receives structured validation errors and is bounded; do not relax constraints until something parses. For tool calls, authorization is evaluated at execution time against current state.
* **Production Reality & Tradeoffs:** Grammars add token-mask overhead and may exclude valid flexible outputs. Schema success is not correctness. Semantic validators require domain code/data and can race state changes. Version every layer together.
* **Red Flag:** Claiming JSON Schema guarantees truthful or authorized output, or using retries as a substitute for a grammar/parser.

