### Q: Diagnose LLM loss spikes, NaNs, divergence after resume, dead experts, and validation regressions.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Freeze the failing step and locate the first divergence across data, activations, loss, gradients, collectives, optimizer/scaler, and checkpoint state. Compare all ranks to the last healthy checkpoint before changing training knobs.
* **The Deep Dive:** Capture step, token/sample IDs, model/optimizer/scheduler/scaler/RNG/data-loader state, and per-rank traces. Check input corruption, extreme lengths, masks, duplicate documents, and loss-token count. Instrument residual/attention-logit/norm activations, loss terms, gradient/update/parameter norms, non-finite counts, clipping, Adam moments, learning rate, and communication timing by layer/rank. The first abnormal tensor is causal evidence; global NaN after all-reduce is not.

Resume-only divergence points to missing optimizer/scaler/RNG/data cursor, schedule step, changed world-size/sharding, or non-atomic checkpoint. MoE dead experts require routing entropy, assignment/capacity/drop, per-expert gradient, and all-to-all diagnostics. Validation regression with healthy training loss suggests distribution/contamination changes, overtraining/repetition, eval serialization, or forgotten capabilities.

```text
replay exact batch -> forward bisect -> loss term -> backward bisect -> optimizer update
        compare healthy checkpoint/ranks at every boundary
```
* **Production Reality & Tradeoffs:** Anomaly mode can perturb timing and memory; use it for minimal replay. Lowering LR/clipping may hide bad data/kernel corruption. At scale, hardware/link errors imitate software instability. Recovery must restart from a verified complete checkpoint and quarantine the trigger.
* **Red Flag:** Blaming mixed precision or lowering learning rate without identifying the first bad rank/layer/state transition.

