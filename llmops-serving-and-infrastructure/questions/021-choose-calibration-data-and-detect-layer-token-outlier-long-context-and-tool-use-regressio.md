### Q: Choose calibration data and detect layer, token, outlier, long-context, and tool-use regressions.
* **Difficulty:** Principal
* **Category:** Quantization
* **The 10-Second Pitch:** Calibration must represent activation ranges and critical behaviors across domains, lengths, tokens, modalities, and tool schemas. Analyze layer-wise error and end-to-end slices, not one average benchmark.
* **The Deep Dive:** Collect prompts with rare tokens, multilingual/code/math, long contexts, high dynamic range, MoE routing, structured outputs, and tool calls. Compare activation distributions, clipping/saturation, cosine/output error per layer, logit KL, perplexity, retrieval needles, generation, and exact schema/argument accuracy. Sensitivity tests keep problematic layers higher precision or adjust group/scales.
* **Production Reality & Tradeoffs:** Calibration leakage and small sets overfit. Long-context KV error can appear only late. Safety/refusal thresholds can shift under tiny logit changes.
Calibration must match activation distributions: system/chat templates, prompt and output lengths, languages, code, tool JSON, multimodal tokens, and rare outliers. Inspect per-layer/channel ranges, clipping, saturation, and error accumulation across decode positions. Evaluate perplexity plus task, calibration, structured-output, long-context retrieval, and safety. A small generic text calibration set may look fine on average while destroying one projection’s outlier channel and causing rare catastrophic tokens.

* **Red Flag:** Calibrating on a handful of short English sentences.
