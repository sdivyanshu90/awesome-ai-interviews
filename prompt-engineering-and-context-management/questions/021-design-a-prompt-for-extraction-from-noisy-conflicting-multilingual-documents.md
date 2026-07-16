### Q: Design a prompt for extraction from noisy, conflicting, multilingual documents.
* **Difficulty:** Senior
* **Category:** Prompt Engineering
* **The 10-Second Pitch:** Extract into a typed schema with source spans, language, confidence/status, and conflict sets; prohibit guessing, preserve original text and normalized values separately, and validate against OCR/layout/domain rules.
* **The Deep Dive:** Provide task/schema, field definitions, units/date/number locale rules, and allowed statuses: `found`, `missing`, `conflicting`, `illegible`, `not_applicable`. Delimit each document with source/version/page/region/language provenance. Instruct the model to cite exact spans for every value, keep original string plus normalized representation, and emit all materially conflicting candidates rather than selecting silently. Cross-document precedence—signed over draft, newer effective date, authoritative source—belongs in explicit deterministic policy where possible.

```json
{"field":"total","status":"conflicting","candidates":[
  {"raw":"1.234,50 €","value":1234.50,"source":"docA:p2:b7"},
  {"raw":"€1,284.50","value":1284.50,"source":"docB:p1:b3"}
]}
```

Precede prompt with OCR/layout outputs retaining coordinates; do not translate away evidence. Post-validate types, checksum/totals, date ordering, currency, required fields, and citation entailment. Route low-confidence/high-impact conflicts to bilingual/domain reviewers.
* **Production Reality & Tradeoffs:** Long documents need region retrieval/tiling and can omit global context. Model confidence is not calibrated. OCR errors and locale ambiguity require source-image review. Avoid logging sensitive documents.
* **Red Flag:** Prompting “extract the correct value” with no missing/conflict/provenance behavior.

