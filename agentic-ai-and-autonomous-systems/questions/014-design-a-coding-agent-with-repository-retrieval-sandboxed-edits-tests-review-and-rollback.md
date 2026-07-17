### Q: Design a coding agent with repository retrieval, sandboxed edits, tests, review, and rollback.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Start from a pinned repository state in a sandbox, gather scoped code context, plan an edit, apply a minimal diff, run targeted then broader checks, inspect failures, and submit the patch plus evidence for review. The agent never writes to protected branches or executes with production secrets.
* **The Deep Dive:** State includes commit SHA, task contract, changed files, commands, outputs, test status, and budget. Tool policy restricts filesystem paths, network, dependency installation, and destructive commands. Use deterministic linters/tests as verifiers; the model can revise only after attaching failure evidence. Human approval gates merges and risky migrations.
The agent first reads repository instructions, builds a symbol-aware context, and proposes a patch plan. Edits occur in a clean sandbox/worktree under path permissions; tests progress from targeted to broader suites. The deliverable is a diff plus test evidence, not direct deployment. Block secret exfiltration, destructive commands, unapproved dependency/network changes, and writes outside scope. On failure, preserve logs and revert only the agent’s own changes—never overwrite unrelated user work.

* **Production Reality & Tradeoffs:** Tests can be incomplete or flaky; distinguish infrastructure failure from code regression. Prompt/context selection is critical—retrieve symbols and call paths rather than dumping the full repository.
* **Red Flag:** Judging success by whether generated code looks plausible without executing tests or reviewing the diff.
