### Q: Explain LLM04 Data and Model Poisoning across pretraining, tuning, preference, RAG, and memory.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Poisoning corrupts behavior through malicious or biased training/retrieval/memory data. It can target availability, broad integrity, or hidden triggers/backdoors.
* **The Deep Dive:** Track source lineage and trust, isolate untrusted uploads, deduplicate, scan anomalies/triggers, review high-impact examples, and compare data/model versions. Preference poisoning manipulates chosen/rejected signals; RAG poisoning creates authoritative-looking chunks; memory poisoning persists false instructions/facts. Use source-weight caps, robust training, retrieval authority/freshness, canaries, and behavioral red teams.
* **Production Reality & Tradeoffs:** Sophisticated clean-label backdoors evade simple filters. Removing source rows does not erase trained influence. Immutable snapshots enable forensic comparison.
Poisoning can target availability, broad quality, a trigger backdoor, retrieval ranking, preference reward, or agent memory. Track lineage and contributor concentration; deduplicate; quarantine anomalous sources; evaluate clean and trigger slices; and require review for high-impact adapters/index updates. Because clean-label examples may appear correct individually, detection needs influence, cluster, temporal, and behavior analysis. Rollback requires immutable dataset/index/checkpoint versions and deletion propagation.

* **Red Flag:** Scanning text for profanity and declaring the dataset unpoisoned.
