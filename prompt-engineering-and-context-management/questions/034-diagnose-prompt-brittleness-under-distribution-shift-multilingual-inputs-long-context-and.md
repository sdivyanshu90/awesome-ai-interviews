### Q: Diagnose prompt brittleness under distribution shift, multilingual inputs, long context, and adversarial text.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Create controlled paired perturbations, trace exact serialization and stage outputs, and localize sensitivity to task ambiguity, tokenizer/language, position/truncation, examples/retrieval, model version, or policy—then replace prompt-only assumptions with structured controls.
* **The Deep Dive:** Start from failing production cases and a frozen bundle. Build metamorphic pairs: paraphrase/format/whitespace, language/dialect/code-switch, entity/number substitution, length/evidence position, distractor insertion, role-like untrusted text, Unicode/encoding, and missing/conflicting data. Some transformations should preserve output; counterfactual fact swaps should change it. Measure flip rate, task/slice quality, schema, refusal, citations, tokens, and latency over samples.

Inspect rendered bytes/tokens, role ordering, truncation, demonstrations, retrieved ranks, tool schemas/results, cache, model/decoding, parser/policy. Remove one component or swap oracle retrieval/translation to attribute failure. Multilingual issues may be fertility/data quality or prompt language; long-context issues may be context construction rather than model capacity; adversarial failures require privilege reduction, not another phrase.

Add minimized variants to hidden regression sets and fix with canonical structured input, clearer contracts/examples, retrieval/routing, constrained decoding, validators, or deterministic policy.
* **Production Reality & Tradeoffs:** Robustness transformations can change semantics across cultures/languages; use native review. Excess prompt rules can reduce robustness through dilution. Revalidate every model/provider change.
* **Red Flag:** Patching the single failing string or adding it to a blocklist without causal localization.

