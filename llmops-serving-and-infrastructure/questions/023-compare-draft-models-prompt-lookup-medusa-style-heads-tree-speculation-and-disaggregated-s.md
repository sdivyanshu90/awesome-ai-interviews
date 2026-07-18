### Q: Compare draft models, prompt lookup, Medusa-style heads, tree speculation, and disaggregated speculation.
* **Difficulty:** Principal
* **Category:** Inference
* **The 10-Second Pitch:** Speculation sources differ: small model predictions, repeated prompt n-grams, extra future-token heads, branching trees, or remote draft workers. Choose by acceptance, added parameters/compute, KV handling, and scheduler integration.
* **The Deep Dive:** Draft model is general but requires serving another model. Prompt lookup is nearly free for copying/repetition but narrow. Multi-token heads share backbone and predict future positions, requiring training and tree verification. Tree methods branch candidate sequences and verify many in one target pass but expand tensors/cache. Disaggregated speculation can use spare/different hardware but adds network/coordination. All must handle rejection and committed KV correctly.
* **Production Reality & Tradeoffs:** Optimize end-to-end goodput by workload. Extra candidates increase memory and can starve normal batching. Structured constraints/penalties must match draft/target semantics.
Prompt lookup drafts sequences found in the prompt; small draft models trade extra compute for acceptance; Medusa-like heads predict multiple future candidates from target hidden state; tree speculation verifies branches; disaggregated drafts use separate resources. Compare exact distribution guarantees, KV handling, tokenizer compatibility, scheduling interference, and batching. High standalone acceptance can reduce fleet throughput if draft work competes for the same bandwidth or variable verification shapes fragment batches.

* **Red Flag:** Choosing the highest standalone draft accuracy regardless of cost.
