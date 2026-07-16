### Q: When should reasoning be replaced by retrieval, calculators, code, formal solvers, or deterministic rules?
* **Difficulty:** Senior
* **Category:** System Design
* **The 10-Second Pitch:** Use the model for interpretation and synthesis; delegate facts, arithmetic, execution, constraint satisfaction, and authorization to systems with stronger guarantees whenever errors are measurable or high impact.
* **The Deep Dive:** Retrieval is appropriate for external/fresh/attributable knowledge; a database/API is stronger for authoritative current state. Calculators handle numeric expressions and units. Sandboxed code handles algorithms, data transforms, simulation, and reproducible artifacts. SAT/SMT/ILP/theorem solvers handle formal constraints/proofs when the problem can be encoded. Deterministic rules enforce business policy, permissions, thresholds, and invariants. The model can parse intent, propose a query/program/formula, explain verified results, and ask clarification.

Decision factors: can the task be specified; is an oracle/verifier available; consequence and reversibility; input sensitivity; latency/cost; tool reliability; and need for evidence. Typed proposals are parsed and semantically validated, executed with least privilege, then results are checked before re-entering context as untrusted data.

```text
natural-language intent -> typed plan -> deterministic/retrieval tool -> verified result -> explanation
```
* **Production Reality & Tradeoffs:** Tools introduce latency, outages, injection, schema mismatch, and sandbox risk. Formalization can encode the wrong problem. Define timeouts/fallbacks and never silently use model arithmetic when the tool fails.
* **Red Flag:** Letting the model decide authorization or claiming reasoning text is equivalent to executing/verifying a computation.

