### Q: Compare frozen-encoder/frozen-LLM training with end-to-end multimodal pretraining.
* **Difficulty:** Principal
* **Category:** Training
* **The 10-Second Pitch:** Freezing preserves strong unimodal capabilities and trains cheaply through a connector; end-to-end tuning adapts representations jointly but costs far more and risks catastrophic forgetting. Staged unfreezing is a common compromise.
* **The Deep Dive:** With both backbones frozen, gradients update projector/resampler/cross-attention only; this works if visual features already contain needed information and LLM can consume it. Unfreezing the vision encoder improves task-specific detail; unfreezing the LLM changes language grounding and generation. Layer-wise LR, modality data mixture, replay text, and alignment losses control drift. End-to-end pretraining can learn deeper fusion unavailable to a small connector.
* **Production Reality & Tradeoffs:** Compare target gains with language-only, vision-only, safety, calibration, and latency regressions. Frozen systems still require compatible normalization/tokenization. Full tuning demands distributed optimizer/activation memory.
Freezing both towers and training only a bridge is cheap but limited to representations already exposed by the towers. Unfreezing the projector plus selected vision/LLM layers improves alignment; full end-to-end training offers capacity but risks language forgetting and unstable modality balance. Stage training from alignment pairs to instruction/grounding data, with separate learning rates and retained text-only evaluation. “Frozen” still incurs activation compute unless features are precomputed.

* **Red Flag:** Assuming frozen models cannot learn multimodal behavior or that full tuning is always superior.
