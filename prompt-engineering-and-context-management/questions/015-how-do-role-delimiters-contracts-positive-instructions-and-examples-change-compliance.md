### Q: How do role, delimiters, contracts, positive instructions, and examples change compliance?
* **Difficulty:** Mid
* **Category:** Prompt Engineering
* **The 10-Second Pitch:** Roles communicate authority, delimiters type regions, explicit contracts define outputs/failures, positive instructions specify desired action, and examples operationalize ambiguous rules. They improve behavior but are probabilistic cues, not enforcement.
* **The Deep Dive:** Use the highest available role only for stable policy/application instructions; put user intent in user scope and external content inside labeled untrusted regions. Delimiters reduce accidental blending and make parsing/provenance clear, but malicious content remains visible to the model. A contract names task, inputs, allowed evidence, output schema, missing/conflict behavior, constraints, and refusal/clarification conditions. Positive instructions such as “extract only claims supported by cited spans” are more actionable than a long list of “do nots.” Examples demonstrate boundary cases and exact formatting, but can override prose through pattern bias or import mistakes.

```text
role authority -> task contract -> typed untrusted inputs -> examples -> query -> constrained output
```

Remove contradictory/redundant rules, state priority, and reserve tokens. Validate using prompt perturbations, adversarial data, multilingual inputs, and missing/conflicting evidence. Deterministic schema/authorization/safety checks remain outside the prompt.
* **Production Reality & Tradeoffs:** More instruction text can lower compliance through dilution/truncation. Examples add cost and contamination. Provider role semantics differ. Keep contracts versioned and concise, and measure per-clause compliance.
* **Red Flag:** Treating roles or XML delimiters as security boundaries, or writing only prohibitions with no valid alternative behavior.

