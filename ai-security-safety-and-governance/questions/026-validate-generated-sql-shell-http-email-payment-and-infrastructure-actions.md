### Q: Validate generated SQL, shell, HTTP, email, payment, and infrastructure actions.
* **Difficulty:** Principal
* **Category:** Security
* **The 10-Second Pitch:** Parse to typed domain-specific commands, allowlist operations/resources, enforce identity and invariants, preview consequential effects, and execute idempotently through narrow services.
* **The Deep Dive:** SQL validates AST, tables/columns, row limits, parameterization, and read/write role. Shell should be replaced by fixed APIs/argv allowlists. HTTP validates scheme/DNS/IP/redirect/host/body/egress. Email validates recipients/classification and approval. Payments enforce account, amount, currency, velocity, and idempotency. Infrastructure uses plan/diff, policy-as-code, scoped credentials, and staged rollout.
* **Production Reality & Tradeoffs:** Dry runs can differ from execution due to races; revalidate preconditions. Validation errors return structured safe feedback, not invite arbitrary repair.
Translate model output into a narrow typed plan before execution. SQL uses parsed ASTs, allowlisted schemas, read/write separation, row limits, and parameter binding; HTTP validates resolved destination through redirects; email/payment/infra changes require recipients/resources, amount/diff, current version, preview, and authorization. Reject ambiguous units and unknown fields. Dry run is evidence, not approval. Record canonical intent, policy decision, executor identity, idempotency key, and effect receipt.

* **Red Flag:** Checking only JSON schema before executing high-impact actions.
