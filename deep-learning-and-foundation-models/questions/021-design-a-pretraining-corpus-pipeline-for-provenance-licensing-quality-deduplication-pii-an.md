### Q: Design a pretraining corpus pipeline for provenance, licensing, quality, deduplication, PII, and mixture weighting.
* **Difficulty:** Principal
* **Category:** Data
* **The 10-Second Pitch:** Build an immutable lineage graph from governed source snapshots through deterministic parsing, safety/PII/quality filtering and split-aware deduplication into versioned weighted manifests; every output token must be traceable and deletable.
* **The Deep Dive:** At acquisition record source URI/snapshot/hash, license/terms, consent/purpose, jurisdiction, timestamp, and access policy. Sandbox parsers, normalize while preserving raw lineage, detect language/domain/format, and attach quality/safety/PII/secret scores rather than destructively losing evidence. Deduplicate before splits using exact hashes, normalized text, MinHash/LSH, semantic/code/image methods, and group related documents; document/paragraph/chunk duplicates need separate handling. Detect benchmark contamination with exact, fuzzy, semantic, and canary/temporal tests.

```mermaid
graph LR
    S[Governed snapshots] --> P[Sandbox parse + normalize]
    P --> F[PII safety quality filters]
    F --> D[Exact + near dedup groups]
    D --> M[Versioned mixture manifest]
    M --> T[Tokenize + deterministic shards]
    T --> C[Training cursor/checkpoint]
```

Mixture weights are a sampling policy over domains/languages/quality, not raw dataset sizes; use temperature/caps and validate effective tokens. Freeze tokenizer and sharding, log each sample’s record/version, and keep deletion maps from source to shards/checkpoints/models.
* **Production Reality & Tradeoffs:** Aggressive quality filtering homogenizes language and removes rare expertise; dedup can erase legitimate repeated conventions. PII detectors miss context and over-redact. Reweighting causes repetition. Publish mixture, coverage, effective epochs, and rights limitations; define retraining/unlearning response.
* **Red Flag:** Scraping first and adding source URLs later, or splitting before deduplication so benchmark variants leak across sets.

