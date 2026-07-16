### Q: Design a prompt for safe code generation and minimal repository edits.
* **Difficulty:** Senior
* **Category:** Prompt Engineering
* **The 10-Second Pitch:** Constrain scope, repository state, allowed paths/APIs, invariants, validation commands, and output as a minimal diff; require the model to inspect before editing and never grant execution or authorization through prompt text.
* **The Deep Dive:** Include task/acceptance criteria, pinned commit, relevant code/style/testing context, files allowed/forbidden, compatibility/security constraints, and a budget on changed files/lines. Ask first for a concise plan and unknowns, then a unified diff or typed edit operations—not rewritten whole files. Require preservation of unrelated changes, no generated binaries/secrets/dependency upgrades unless requested, and explicit assumptions. The executor independently parses the patch, blocks path traversal/binaries/large deletes, applies to an ephemeral worktree, and runs formatter, typecheck/build, targeted then broader tests and security scans in a sandbox.

```text
inspect pinned repo -> propose minimal plan -> emit diff -> policy-check paths/size
-> apply sandbox -> tests/scans -> present diff + evidence -> human merge
```

Repair iterations receive actual diagnostics and remain bounded. The prompt cannot make shell/network/package access safe; runtime has no ambient credentials and default-deny egress.
* **Production Reality & Tradeoffs:** Overly narrow context misses dependencies; too much repo context leaks secrets and costs tokens. Tests are incomplete. Require review for migrations/security/auth. Indexes must be commit-aware.
* **Red Flag:** Telling the model “be careful” while allowing arbitrary shell/filesystem writes on the developer host.

