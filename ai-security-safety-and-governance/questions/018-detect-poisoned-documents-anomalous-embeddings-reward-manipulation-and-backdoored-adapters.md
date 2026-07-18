### Q: Detect poisoned documents, anomalous embeddings, reward manipulation, and backdoored adapters.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Combine provenance/trust scoring, distribution and influence diagnostics, behavioral canaries, and quarantine/review. Detection must cover data geometry and downstream behavior.
* **The Deep Dive:** Documents: detect duplicates, sudden authority/link changes, hidden instructions, semantic inconsistency, and source anomalies. Embeddings: monitor norms, clusters, hubness, neighborhood takeover, and index deltas. Reward data/models: inspect annotator/source patterns, score distributions, disagreement, and adversarial high-score outputs. Adapters: verify origin/base compatibility, tensor statistics, activation behavior, trigger suites, and sandbox loading.
* **Production Reality & Tradeoffs:** Adaptive attacks imitate normal statistics. False positives target minority/domain data. Maintain immutable snapshots and rollback; restrict who can publish artifacts.
Detection combines source/lineage anomalies, duplicate and influence analysis, embedding-neighborhood shifts, spectral/activation signatures, trigger sweeps, and behavioral canaries. Reward manipulation appears as annotator/source concentration or proxy-specific gains; backdoored adapters require base-hash/signature checks plus clean/trigger evaluation. No detector proves absence. Keep ingestion staged, train reproducibly, compare ablations removing suspicious clusters, and preserve a known-good rollback path.

* **Red Flag:** Relying on antivirus-style string signatures for model/data poisoning.
