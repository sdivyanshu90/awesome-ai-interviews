### Q: Design an embedding-model/index migration with shadow reads, backfill, and zero downtime.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Version embeddings and indexes immutably, dual-write new content, backfill historical data, shadow production queries against both, evaluate neighbor/end-to-end changes, canary routing, then retire old state only after rollback windows.
* **The Deep Dive:** A registry binds model/tokenizer/instruction, dimension, normalization, distance, index parameters, corpus snapshot, and schema. Backfill is idempotent and prioritizes hot/current documents. During migration, query both old and new indexes or route by version; never mix incompatible vectors in one metric space. Compare recall, nDCG, citation support, latency, memory, filters, and zero-result rates. Reconcile updates/deletes that occur during backfill from an ordered change log.
* **Production Reality & Tradeoffs:** Dual storage/compute can be expensive. Cache keys include embedding/index version. Rollback requires old query embedding service and index to remain available until confidence is established.
Backfill and live mutation form a dual-write problem. Capture a high-water mark, backfill the snapshot, consume the ordered change log after that mark, then reconcile counts/hashes before canary. Shadow queries must use identical authorization and log paired candidate sets without exposing new results. Cache/index routing carries a version. Retirement waits for deletion parity, rollback expiry, and proof that no clients still produce old-space query vectors.

* **Red Flag:** Overwriting vectors in place and hoping ranking stays compatible.
