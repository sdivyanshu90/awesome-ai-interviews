### Q: Evaluate injection and jailbreak robustness without turning a static benchmark into a false guarantee.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Use adaptive multi-turn attacks across modalities and system effects, hidden rotating suites, human red teams, and measured attack success/false-positive cost. Evaluate the full application, not only model text.
* **The Deep Dive:** Cover direct/indirect/stored injection, obfuscation, tool-result payloads, secret requests, unauthorized writes, data exfiltration, and benign boundary cases. Vary language, format, context position, and attacker knowledge. Score whether protected assets/actions were reached, which control stopped the path, and user utility. Hold out templates and continuously add incidents. Re-run after model/prompt/tool/index changes.
* **Production Reality & Tradeoffs:** Publishing attacks leads to overfitting; maintain private sets. Automated attacks miss creativity and humans miss scale. No finite pass proves security.
Build adaptive tests from an abuse taxonomy and mutate language, encoding, placement, modality, multi-turn timing, and retrieved context. Measure attack success rate with confidence intervals plus benign refusal/utility cost. Keep a sequestered holdout and rotate attacks after remediation. Static pass rates overestimate security because attackers observe outputs and iterate. Evaluate the deployed model, template, retrieval, tools, and policies together; component-only refusal tests miss executor compromise.

* **Red Flag:** Reporting 0% jailbreak on one public list as secure.
