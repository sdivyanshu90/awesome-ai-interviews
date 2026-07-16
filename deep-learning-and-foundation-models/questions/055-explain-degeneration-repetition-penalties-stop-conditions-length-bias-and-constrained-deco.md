### Q: Explain degeneration, repetition penalties, stop conditions, length bias, and constrained decoding.
* **Difficulty:** Senior
* **Category:** Inference
* **The 10-Second Pitch:** Generation failures arise from model probabilities plus decoding: loops and blandness, heuristic penalties that distort logits, EOS/stop-string ambiguity, cumulative log-probability length bias, and constraints that must preserve only valid continuations.
* **The Deep Dive:** Greedy/beam can enter high-probability loops; high temperature produces incoherence; low temperature/top filters collapse diversity. Repetition penalties rescale logits of seen tokens, presence/frequency penalties subtract by occurrence, and n-gram bans hard-mask repeats—none understands legitimate repetition and all change probability semantics. Stop conditions include EOS token, max tokens/time/cost, structured grammar completion, tool-call boundary, and user cancellation. Stop strings require incremental byte/token matching because strings cross token boundaries; decide whether delimiter is included.

Beam scores sum log probabilities, inherently becoming more negative with length; length normalization $s/((5+T)/6)^\alpha$ or average log-prob changes ranking but can favor verbosity. Constrained decoding maintains a DFA/parser state and masks tokens whose byte strings cannot extend a valid JSON/regex/grammar path; token-level masks must account for partial terminals and UTF-8. Semantic/business validity still needs external validation.

```text
logits -> repetition/constraints -> temperature/filter -> token -> update stop + grammar state
```
* **Production Reality & Tradeoffs:** Penalties/constraints add per-token latency and can create no-valid-token states. Schema-constrained syntax does not authorize tool actions. Tune on task distributions and report truncation/loop/validity rates, not anecdotes.
* **Red Flag:** Using stop strings as if they align with token boundaries, or claiming valid JSON means a valid/safe action.

