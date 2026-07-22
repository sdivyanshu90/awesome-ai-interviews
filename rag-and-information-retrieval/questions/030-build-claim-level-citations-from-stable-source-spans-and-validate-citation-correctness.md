### Q: Build claim-level citations from stable source spans and validate citation correctness.
* **Difficulty:** Senior
* **Category:** RAG
* **The 10-Second Pitch:** Represent retrieved evidence as stable document-version spans, require material claims to reference span IDs, then validate existence, authorization, entailment, and completeness. Citation formatting is the final rendering step, not evidence generation.
* **The Deep Dive:** Ingestion stores source ID, immutable version/hash, page/section, byte or token offsets, and text. Prompt exposes compact span IDs. Parse answer into atomic claims and citations. Validators ensure cited spans were supplied and accessible; NLI/LLM/human checks assess whether each span supports the claim; completeness checks uncited material claims. UI resolves IDs to quoted snippets and source versions. Derived/table claims may need multiple citations and transformation metadata.

Citation correctness has two directions: precision—does each cited span actually support its claim—and completeness—does every material claim carry a citation. Validators must also handle temporal qualifiers and unit conversions, not only surface entailment. If OCR or rechunking shifts offsets, maintain a version resolver that maps old span IDs to structural anchors rather than silently pointing stale citations at new text.
* **Production Reality & Tradeoffs:** Documents can change, so citations pin versions. Automatic entailment is imperfect, especially for numbers/negation; high-stakes workflows need deterministic checks or review. More citations can reduce readability.
* **Red Flag:** Counting citation markers without checking support.
