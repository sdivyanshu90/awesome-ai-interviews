### Q: Secure MCP/tool/plugin registries against typosquatting, schema substitution, and malicious updates.
* **Difficulty:** Principal
* **Category:** Supply Chain
* **The 10-Second Pitch:** Use curated identities, signed immutable versions, schema/capability review, allowlisted sources, dependency scanning, staged rollout, and runtime least privilege. Tool name/description is untrusted metadata.
* **The Deep Dive:** Registry verifies publisher, artifact digest/signature, manifest, endpoint identity, requested permissions, data handling, and version compatibility. Pin versions; diff schema/description/capabilities on update; require approval for privilege expansion. Detect lookalike names. Sandbox connectors and restrict egress/secrets. Canary and support revocation/rollback. Bind cached tool calls/prompts to schema version.
* **Production Reality & Tradeoffs:** A signed plugin can still be malicious. Dynamic discovery expands attack surface and prompt injection through descriptions. High-risk deployments may forbid user-installed tools.
Treat registry identity, schema, and artifact digest as one signed release. Reserve namespaces, verify publisher ownership, pin versions/digests, review permission diffs, generate SBOM/provenance, and stage updates through sandboxed compatibility and adversarial tests. At runtime, allowlist tool IDs and verify response schemas; do not auto-install a model-suggested name. Typosquatting and schema substitution can preserve a familiar description while redirecting credentials or widening effects.

* **Red Flag:** Auto-installing the closest-name tool requested by the model.
