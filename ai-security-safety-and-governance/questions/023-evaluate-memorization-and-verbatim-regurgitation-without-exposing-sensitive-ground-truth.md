### Q: Evaluate memorization and verbatim regurgitation without exposing sensitive ground truth.
* **Difficulty:** Principal
* **Category:** Privacy Evaluation
* **The 10-Second Pitch:** Use controlled canaries, access-restricted similarity evaluation, exposure metrics, and red-team prompts; report aggregate risk without publishing secrets or extractable examples.
* **The Deep Dive:** Insert unique consented canaries at known repetition levels in controlled training, then measure rank/exposure. For real sensitive data, compute exact/near-match inside a secure enclave against authorized corpora and emit counts/risk bands. Test prefix completion, indirect elicitation, multilingual/encoded prompts, and fine-tuning/adapters. Separate legitimate common phrases from rare identifying sequences.
* **Production Reality & Tradeoffs:** Canaries approximate but do not prove real-data privacy. Evaluators/logs can become a new leak. Restrict access and delete extracted outputs.
Use synthetic secrets/canaries with controlled exposure, compare target likelihood or extraction rank against matched nonmembers, and impose strict query/access limits. Store ground truth and generations in a restricted enclave; publish aggregates after privacy review. Exact-match alone misses transformed regurgitation, while fuzzy matching can create false accusations. Validate suspicious matches against corpus provenance and uniqueness. Never seed production models with real sensitive strings merely to make a memorization benchmark.

* **Red Flag:** Publishing leaked examples to demonstrate the model leaks.
