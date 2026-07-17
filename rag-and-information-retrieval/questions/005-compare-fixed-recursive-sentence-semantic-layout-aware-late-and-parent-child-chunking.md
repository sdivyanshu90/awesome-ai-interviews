### Q: Compare fixed, recursive, sentence, semantic, layout-aware, late, and parent-child chunking.
* **Difficulty:** Senior
* **Category:** RAG
* **The 10-Second Pitch:** Chunking defines the retrieval unit and evidence boundary. Fixed methods are simple; sentence/recursive respect text; semantic follows topic shifts; layout preserves documents/tables; late chunking contextualizes chunks; parent-child retrieves small spans but supplies larger context.
* **The Deep Dive:** Fixed token windows give predictable index/storage but cut sentences/tables and mix sections. Overlap reduces boundary loss but duplicates hits, index size, and context. Recursive splitters prefer headings/paragraphs/sentences then enforce a size. Sentence chunks are precise but often lack context. Semantic chunking detects embedding/discourse changes, adding model cost and instability. Layout-aware parsing retains page, bounding boxes, reading order, list/table/figure relationships. Late chunking encodes a larger document context then pools token spans so embeddings know surrounding content; it is expensive and model-length-limited. Parent-child indexes small child chunks for recall and returns a parent/neighbor window for generation.

```text
document structure -> retrievable child span -> optional parent/neighbor expansion -> cited source span
```

Preserve document/version/page/section and exact offsets. Deduplicate overlapping results and ensure citations point to minimal evidence. Evaluate chunk sizes/overlap on retrieval recall, context precision, complete-answer support, citation span, tokens, and latency; no universal size.
* **Production Reality & Tradeoffs:** Large chunks improve context but dilute embeddings and waste prompt tokens; small chunks improve precision but lose relations. Rechunking requires re-embedding/index migration and invalidation. Tables/code need specialized units.
* **Red Flag:** Choosing 500 tokens with 20% overlap by habit and evaluating only answer fluency.

