### Q: How do stop sequences, logit bias, sampling, retries, and repair loops affect structured reliability?
* **Difficulty:** Senior
* **Category:** Inference
* **The 10-Second Pitch:** These controls reshape generation but do not replace parsing: stop strings can split across tokens, logit bias is tokenizer-specific, sampling trades determinism, and retry/repair must be bounded, idempotent, and driven by validation errors.
* **The Deep Dive:** Stop tokens are checked on IDs; stop strings require incremental byte/text matching because delimiters can span tokens and appear inside escaped content. Decide whether to include them and how to handle streaming rollback. Logit bias changes selected token logits but a visible string has multiple tokenizations, and extreme bias can create degenerate alternatives. Temperature/top-p affect syntax and semantic diversity; temperature zero is still not guaranteed deterministic across serving implementations.

On failure, classify parse, schema, semantic, policy, timeout, and truncation errors. A repair prompt receives the original structured candidate plus machine error and must preserve intent; cap attempts/tokens/time and avoid executing any partial side effect. Retries use same operation ID and deadline, not fresh uncontrolled loops. Grammar-constrained decoding is preferable for strict syntax; validators remain mandatory.

```text
generate -> stream/stop -> parse -> validate
              failure -> bounded targeted repair -> validate -> reject/abstain
```
* **Production Reality & Tradeoffs:** Retries multiply tail latency/cost and correlated outputs often repeat errors. Logit biases break across tokenizer/model versions. Stop strings can enable truncation attacks. Monitor first-pass validity, repaired validity, semantic failures, and attempts.
* **Red Flag:** Retrying “please fix JSON” indefinitely or using stop strings as a safe parser.

