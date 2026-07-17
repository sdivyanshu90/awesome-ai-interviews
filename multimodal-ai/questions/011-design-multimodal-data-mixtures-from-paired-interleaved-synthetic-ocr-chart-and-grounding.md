### Q: Design multimodal data mixtures from paired, interleaved, synthetic, OCR, chart, and grounding data.
* **Difficulty:** Principal
* **Category:** Data
* **The 10-Second Pitch:** Define capabilities first, then mix data by token/sample quality and task need while preserving provenance, licensing, deduplication, safety, and modality balance. Paired captions teach alignment; interleaved data teaches discourse; grounding/OCR/chart data teaches precision.
* **The Deep Dive:** Normalize media, retain source metadata, detect image and caption duplicates, and prevent train-test leakage through near-duplicate visuals. Synthetic captions/instructions expand coverage but transfer teacher errors. OCR requires exact text and regions; charts require underlying values/relationships; grounding needs phrase-box/mask links. Use sampling weights, curricula, and loss masks so abundant low-quality captions do not dominate scarce precise labels.
* **Production Reality & Tradeoffs:** Image rights, faces, location, and sensitive documents raise privacy issues. Monitor capability by data source and ablate mixture weights. Dataset size is not equivalent to visual diversity.
A data mixture should target capabilities, not dataset names: global semantics, OCR, spatial grounding, multi-image coreference, charts/tables, long interleaving, safety, and abstention. Normalize licenses/provenance, deduplicate within and across modalities, and prevent image derivatives from crossing splits. Synthetic captions increase coverage but transfer teacher hallucinations. Mixture weights should be token/pixel-compute aware; high-resolution OCR examples cost far more than short captions and can dominate optimization.

* **Red Flag:** Mixing all multimodal data uniformly and evaluating only general VQA.
