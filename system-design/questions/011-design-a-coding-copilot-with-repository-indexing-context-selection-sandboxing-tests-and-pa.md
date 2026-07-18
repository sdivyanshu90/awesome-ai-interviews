### Q: Design a coding copilot with repository indexing, context selection, sandboxing, tests, and patches.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Build a commit-aware code graph/index, retrieve symbols and dependencies under repo ACLs, generate minimal structured patches, execute in an isolated reproducible sandbox, and return evidence from tests/static analysis.
* **The Deep Dive:** Ingestion binds tenant/repo/commit, parses AST/symbols/imports/call graph, chunks code and docs, and stores lexical plus embedding indexes. Query planner combines open files, diagnostics, symbols, dependency graph, history, and search; context cites paths/lines/commit and excludes secrets/generated binaries. Model proposes a diff, not arbitrary filesystem mutation. Apply to an ephemeral worktree after path/size/binary policy. Sandbox uses pinned toolchain/dependencies, CPU/memory/time/process/network limits, no host credentials, and auditable commands. Run formatter, compile/typecheck, targeted tests selected by dependency impact, broader tests, static/security scans, and patch review. Iterate under bounded attempts and show failures rather than hiding them.
* **Production Reality & Tradeoffs:** Indexes go stale after edits; incremental parsing and commit keys matter. Test execution is expensive and untrusted repositories can attack runners. Semantic retrieval may miss exact identifiers, so hybrid search is essential.
* **Red Flag:** Embedding every file and letting generated shell commands run on the developer host with credentials.

