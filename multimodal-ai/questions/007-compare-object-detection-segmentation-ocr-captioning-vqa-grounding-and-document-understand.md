### Q: Compare object detection, segmentation, OCR, captioning, VQA, grounding, and document understanding objectives.
* **Difficulty:** Senior
* **Category:** Conceptual
* **The 10-Second Pitch:** These tasks predict different structures: boxes/classes, pixel masks, text sequences, descriptions, answers, text-region correspondences, or layout-aware fields. Architecture, loss, annotation, and metrics must match the output.
* **The Deep Dive:** Detection combines classification with box regression and matching/NMS; segmentation predicts semantic or instance masks. OCR includes text detection, recognition, order, and confidence. Captioning is autoregressive conditional generation; VQA may classify or generate answers. Grounding aligns phrases to boxes/masks, often with contrastive and coordinate losses. Document models jointly use OCR tokens, 2D layout, images, tables, and key-value relations.
* **Production Reality & Tradeoffs:** One model can multitask but label mixtures and metric conflicts matter. Exact OCR and coordinates need deterministic postprocessing; generative VQA can hallucinate. Evaluate per task and source type.
These tasks differ in output geometry and loss. Detection predicts classes plus boxes with matching/NMS; semantic segmentation labels pixels; instance segmentation separates objects; OCR combines detection and sequence recognition; captioning uses autoregressive language loss; VQA predicts text/labels conditioned on an image; grounding aligns phrases to boxes/masks; document models additionally preserve reading order and layout. Sharing one encoder is possible, but task heads, supervision density, evaluation, and failure costs remain distinct.

* **Red Flag:** Treating all vision-language tasks as generic caption generation.
