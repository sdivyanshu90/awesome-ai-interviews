### Q: Explain LLM03 Supply Chain risk for weights, datasets, packages, containers, plugins, and endpoints.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** The AI supply chain can introduce malicious code, backdoored models/data, dependency compromise, unsafe serialization, or provider substitution. Pin, verify, isolate, inventory, and continuously scan every artifact and service.
* **The Deep Dive:** Maintain provenance and hashes/signatures for weights/datasets; SBOMs for packages/images; trusted registries; reproducible builds; vulnerability/license scanning; and approval for plugins/tools. Avoid unsafe pickle loading; sandbox model conversion. Authenticate endpoints and verify model/version responses. Test weights/adapters for anomalous behavior and restrict runtime egress/credentials.
* **Production Reality & Tradeoffs:** Signatures prove origin/integrity, not benignness. Transitive dependencies and mutable tags are common gaps. Plan revocation and rollback.
Supply-chain trust covers source and derivation, not merely malware scans. Pin weight/dataset/package/container/plugin digests, verify signatures and provenance attestations, generate SBOMs, scan unsafe serialization, and load artifacts without network or excessive privilege. Reproduce critical builds and record base/model licenses. A signed malicious artifact remains malicious, so combine authenticity with policy, behavior evaluation, and sandboxed loading.

* **Red Flag:** Downloading an unreviewed model/package into a privileged serving process.
