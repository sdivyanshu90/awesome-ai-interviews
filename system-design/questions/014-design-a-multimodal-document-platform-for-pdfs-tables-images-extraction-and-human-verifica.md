### Q: Design a multimodal document platform for PDFs, tables, images, extraction, and human verification.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Preserve page/region provenance through parsing, OCR, layout, tables, and VLM extraction; validate typed outputs against evidence and route low-confidence/high-impact fields to reviewers.
* **The Deep Dive:** Ingestion stores immutable original/version/hash and renders pages in a sandbox. Native text, OCR, layout blocks, reading order, figures, and table structure become a document graph with page/bounding-box provenance. Route born-digital text to parsers, scans to OCR, complex regions to layout/table/VLM models, and fuse duplicates. Extraction uses versioned schemas and produces value, normalized type/unit, confidence, and evidence region. Validators enforce types, totals, cross-field consistency, and business rules. Human review UI shows source crop beside proposed value, supports correction/reason codes, and feeds governed labels. Search combines lexical/vector/structural retrieval; answers cite page/region/table cells. Deletion and document version changes invalidate every derivative.
* **Production Reality & Tradeoffs:** OCR confidence is not business correctness, and VLMs hallucinate plausible fields. Rendering/parsing hostile PDFs requires isolation. High DPI and page tiling improve small text but increase GPU/token cost.
* **Red Flag:** Flattening a PDF to text and discarding layout, table structure, and evidence coordinates.

