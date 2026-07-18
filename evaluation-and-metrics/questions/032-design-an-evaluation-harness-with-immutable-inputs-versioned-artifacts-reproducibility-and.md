### Q: Design an evaluation harness with immutable inputs, versioned artifacts, reproducibility, and release gates.
* **Difficulty:** Principal
* **Category:** Evaluation Platform
* **The 10-Second Pitch:** Represent every run as an immutable manifest linking dataset, system bundle, environment, seeds, judge/rubric, and code; execute in isolated workers, preserve traces, compare with uncertainty, and enforce risk-tiered gates.
* **The Deep Dive:** A run manifest pins dataset snapshot and item hashes; model/provider and decoding; tokenizer, prompt, tools, retrieval index, policy; container/dependencies/hardware; judge/rubric; random seeds; and evaluation code commit. A scheduler shards items into idempotent jobs, workers capture raw structured outputs, timings, token/tool usage, errors, and provenance, and reducers compute metrics with cluster-aware intervals. Artifacts are append-only and content-addressed; sensitive traces use access control, redaction, encryption, and retention. Comparison service supports paired deltas and slice drill-down. Gates distinguish required zero-tolerance invariants, statistical non-regression, minimum capability, and budget/SLO bounds, with explicit waiver owner/expiry.

```text
manifest -> isolated workers -> raw traces -> deterministic reducers -> report/diff -> release gate
    |             |                 |                 |                 |
 versions      retries          provenance        uncertainty       audit/waiver
```
* **Production Reality & Tradeoffs:** Third-party APIs and stochastic models limit bitwise reproducibility; preserve request/response metadata and use statistical replay. Full traces can leak data and cost storage. Gate severity, not every noisy metric.
* **Red Flag:** Saving only aggregate scores without the inputs, system versions, raw outputs, or evaluator versions needed to reproduce them.

