### Q: Compare FID, KID, CLIPScore, precision/recall, human preference, and task-specific evaluation.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** FID/KID compare feature distributions; CLIPScore measures text-image alignment; precision/recall separate fidelity/coverage; humans judge nuanced utility; task metrics evaluate actual downstream constraints. No metric alone establishes quality.
* **The Deep Dive:** FID uses Gaussian Fréchet distance between Inception features and is biased by sample size/preprocessing. KID uses polynomial-kernel MMD with an unbiased estimator. CLIPScore inherits CLIP biases and can reward superficial alignment. Generative precision asks whether samples lie near real manifold; recall asks whether real modes are covered. Human pairwise studies need blind randomization, ties, rater quality, and confidence intervals. OCR, identity, spatial control, and safety require dedicated metrics.
* **Production Reality & Tradeoffs:** Use fixed preprocessing/reference sets and sufficient samples. Benchmark gaming and training overlap invalidate results. Report latency/cost alongside quality.
FID compares Gaussian approximations to feature distributions and is biased by sample count/domain; KID uses an unbiased polynomial-kernel MMD estimator but shares feature dependence. CLIPScore measures prompt-image alignment and can reward superficial shortcuts. Precision/recall separates fidelity from coverage. None proves typography, counting, anatomy, identity preservation, or safety. Use fixed preprocessing and sample counts, confidence intervals, memorization checks, task detectors, and blinded human pairwise preference stratified by prompt type.

* **Red Flag:** Comparing FID values computed with different feature pipelines.
