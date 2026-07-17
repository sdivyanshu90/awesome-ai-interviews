### Q: Diagnose VLM hallucination, counting, spatial reasoning, OCR, chart, and fine-grained grounding failures.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Decompose the pipeline into preprocessing, visual encoding, connector compression, language reasoning, and decoding. Hold components/evidence constant to identify whether information was absent, lost, misgrounded, or incorrectly reasoned.
* **The Deep Dive:** Inspect original versus resized/tiles; test OCR/detector probes on encoder features; vary connector token count; ask extractive region questions before open generation; supply ground-truth OCR/table data to isolate reasoning. Counting fails through duplicate tiles and weak object individuation. Spatial errors expose coordinate/resolution limits. Chart errors may come from OCR, legend mapping, axes, or arithmetic. Grounding needs box/mask evidence, not fluent text.
* **Production Reality & Tradeoffs:** Add targeted data or tools at the failing stage. Higher resolution can worsen cost/duplication. Require abstention and region citations for high-stakes visual claims.
Localize the failure before tuning: OCR depends on native pixel density and tokenizer; counting needs object separation and aggregation; charts require axis/legend binding; spatial reasoning requires coordinate consistency; hallucination often arises when the language prior overwhelms weak visual evidence. Build contrast sets changing only the relevant pixels, require boxes/regions or evidence spans, and measure abstention when evidence is absent. More generic instruction tuning may improve fluency while worsening grounding.

* **Red Flag:** Treating every wrong visual answer as an LLM hallucination.
