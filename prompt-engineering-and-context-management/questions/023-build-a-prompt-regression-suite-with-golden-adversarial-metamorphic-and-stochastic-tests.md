### Q: Build a prompt regression suite with golden, adversarial, metamorphic, and stochastic tests.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Version immutable cases and the full prompt bundle; combine expert gold, production replay, adversarial attacks, and metamorphic invariants; run multiple samples where stochastic and compare paired quality, safety, schema, latency, and cost.
* **The Deep Dive:** Golden cases have evidence-backed expected outputs/rubrics, boundaries, missing/conflict cases, languages, and severity. Production replay represents real lengths/intents after privacy controls. Adversarial sets target injection, truncation, Unicode, malformed tools, secret extraction, and overrefusal. Metamorphic transformations define expected invariance/change: reorder irrelevant fields, paraphrase, rename entities, swap one numeric fact, move evidence, or insert distractors. Group variants by seed case to prevent leakage and bootstrap at group/user level.

Pin model/provider/version, chat template, prompt/examples/tool schemas, retrieval snapshot, decoding, parser/policy, and evaluator. For nondeterministic generation run enough independent seeds, report pass probability/uncertainty and worst severe failures; deterministic schema/security invariants should pass every sample. Validate LLM judges against humans and perturbations. Release gates use paired deltas and zero-tolerance high-impact effects, then shadow/canary.
* **Production Reality & Tradeoffs:** Static suites become tuning targets and stale. Rotate hidden sets while retaining longitudinal anchors. More samples cost money; allocate to high-variance/high-risk cases. Store raw traces securely.
* **Red Flag:** Testing only exact string equality on happy paths or accepting one successful stochastic run.

