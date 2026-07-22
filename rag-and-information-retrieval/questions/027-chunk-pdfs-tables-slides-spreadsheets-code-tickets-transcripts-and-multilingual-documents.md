### Q: Chunk PDFs, tables, slides, spreadsheets, code, tickets, transcripts, and multilingual documents.
* **Difficulty:** Senior
* **Category:** RAG
* **The 10-Second Pitch:** Chunk according to semantic and structural units, not one token window. Preserve layout hierarchy, headers, row/column context, code scope, speaker/time, language, provenance, and parent expansion.
* **The Deep Dive:** PDFs need reading-order and repeated-header removal; tables need schema/header attached to rows and sometimes table-level summaries; slides keep title and spatial groups; spreadsheets preserve sheet/range/formulas; code follows symbols/AST with imports and callers; tickets preserve thread chronology/participants; transcripts use speaker/time/topic segments. Multilingual segmentation must respect script and tokenizer fertility. Index child units for precision and retrieve parents for context.

Preserve structure as typed metadata on each retrieval unit: a table row carries table ID, caption, headers, row/column coordinates, cell text and value, and page span; a code chunk carries symbol, signature, enclosing scope, and commit. Build format-specific gold questions, because aggregate retrieval metrics can hide catastrophic OCR or table-parsing failures confined to one format. Chunk boundaries must stay stable enough for citations to survive reprocessing versions.
* **Production Reality & Tradeoffs:** OCR/layout quality often dominates. Overlap increases duplicates and citation ambiguity. Evaluate answer-support recall and citation spans by document type, then tune separately.
* **Red Flag:** Using a universal 500-token overlapping splitter for every format.
