### Q: Chunk PDFs, tables, slides, spreadsheets, code, tickets, transcripts, and multilingual documents.
* **Difficulty:** Senior
* **Category:** RAG
* **The 10-Second Pitch:** Chunk according to semantic and structural units, not one token window. Preserve layout hierarchy, headers, row/column context, code scope, speaker/time, language, provenance, and parent expansion.
* **The Deep Dive:** PDFs need reading-order and repeated-header removal; tables need schema/header attached to rows and sometimes table-level summaries; slides keep title and spatial groups; spreadsheets preserve sheet/range/formulas; code follows symbols/AST with imports and callers; tickets preserve thread chronology/participants; transcripts use speaker/time/topic segments. Multilingual segmentation must respect script and tokenizer fertility. Index child units for precision and retrieve parents for context.
* **Production Reality & Tradeoffs:** OCR/layout quality often dominates. Overlap increases duplicates and citation ambiguity. Evaluate answer-support recall and citation spans by document type, then tune separately.
Preserve structure as metadata and retrieval units. For a table, store table ID, caption, headers, row/column coordinates, cell text/value, and page span; for code, store symbol, signature, enclosing scope, imports, and commit. Child chunks retrieve precisely, parent expansion supplies context. Build format-specific gold questions because aggregate retrieval can hide catastrophic OCR/table errors. Chunk boundaries must remain stable enough for citations across reprocessing versions.

* **Red Flag:** Using a universal 500-token overlapping splitter for every format.
