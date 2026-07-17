### Q: Evaluate VLMs for answer accuracy, grounding, OCR, robustness, calibration, and safety.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Use a multidimensional suite: task correctness, region/span grounding, exact OCR, perturbation robustness, selective risk, and multimodal safety. Aggregate VQA scores alone hide whether answers are visually supported.
* **The Deep Dive:** Evaluate open answers with expert/validated judges plus exact subsets. Grounding uses IoU/pointing-game and phrase-region consistency. OCR uses CER/WER and layout/table metrics. Perturb resolution, crop, blur, lighting, compression, overlays, and distractor images. Calibration measures coverage versus error under abstention. Safety includes hidden image text, adversarial patches, sensitive inference, faces, and cross-modal policy conflicts.
* **Production Reality & Tradeoffs:** Benchmark contamination and judge bias are material. Test real camera/document distributions and latency by visual-token count. Human review remains necessary for nuanced grounding.
Use separate metrics for answer semantics, evidence localization, OCR, calibration, robustness, and unsafe content. Grounding requires IoU/pointing or phrase-region alignment, not merely a correct noun. Create controlled perturbations—blur, crop, rotation, text size, distractor image, reordered multi-image input—and slice by language and document type. Calibration must include unanswerable images. Human evaluation needs the original-resolution asset and rubric; thumbnails can make evaluators incorrectly agree with model errors.

* **Red Flag:** Using an LLM judge that cannot see the image.
