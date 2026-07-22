### Q: Design a data-analysis agent with governed datasets, executable notebooks, and auditable reports.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Authenticate the analyst, expose cataloged read-only datasets through policy, plan queries, execute code in an isolated reproducible environment, validate results, and generate a report whose claims link to queries, snapshots, and artifacts.
* **The Deep Dive:** A catalog supplies schemas, lineage, classifications, row/column policies, and freshness. The agent proposes a structured analysis plan; query service applies identity and limits. Sandbox pins packages, blocks uncontrolled egress, and stores notebook/code, parameters, dataset snapshot IDs, stdout/errors, and generated figures. Statistical checks cover leakage, missingness, sample size, multiple testing, and uncertainty. Reports cite cells/query hashes and distinguish observations from interpretation. Sensitive exports require approval.

  Keep plan, executable code, results, and narrative as separate artifacts with query/code/data snapshot hashes linking them. Units, joins, and denominators are validated before the model writes conclusions, and every claim must trace to an executed cell rather than model recall; publishing, emailing, or writing data back is a distinct effect gated by its own approval, never a side effect of report generation.

* **Production Reality & Tradeoffs:** LLMs can invent columns or misread correlations. Use schema introspection and execution feedback. Large scans need cost estimates and quotas; derived artifacts inherit source classification.
* **Red Flag:** Letting generated SQL bypass row-level security or presenting unexecuted code results.
