### Q: Secure model artifacts with hashes, signatures, SBOMs, provenance attestations, scanning, and isolated loading.
* **Difficulty:** Principal
* **Category:** Supply Chain
* **The 10-Second Pitch:** Verify immutable artifact digest and trusted signature, retain build/data provenance and SBOM, scan dependencies/format, and load in a process with no ambient credentials or network.
* **The Deep Dive:** Registry metadata binds base/model/tokenizer/config/license, source commit, builder, evaluation, and approvals. Signature verification checks trusted identity and digest before use. SBOM covers code/container/native libraries. Avoid arbitrary-code serialization; convert safely. Sandbox inspection and validate tensor names/shapes/dtypes/finiteness before promotion. Attestations describe build inputs/process.
* **Production Reality & Tradeoffs:** A valid signature can sign malicious content; trust and review still matter. Huge weights make scanning costly. Support revocation and rollback.
A secure artifact pipeline records source digest, build inputs, training data manifest, code, environment, evaluator results, signature, and approval. Verify before load, reject unsafe pickle-like execution, scan tensors/metadata, and initialize in an isolated networkless worker with minimal filesystem access. SBOMs cover software dependencies; model cards cover behavior, neither substitutes for provenance. Rotation and revocation must prevent a previously trusted but compromised digest from re-entering caches.

* **Red Flag:** Loading community pickle weights inside production control plane.
