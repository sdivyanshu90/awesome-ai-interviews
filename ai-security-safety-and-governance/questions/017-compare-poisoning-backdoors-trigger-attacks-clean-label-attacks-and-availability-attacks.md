### Q: Compare poisoning, backdoors, trigger attacks, clean-label attacks, and availability attacks.
* **Difficulty:** Senior
* **Category:** ML Security
* **The 10-Second Pitch:** Poisoning changes training data; backdoors create targeted hidden behavior; triggers activate it; clean-label attacks preserve apparent labels; availability attacks broadly degrade training/service. Taxonomy describes objective and stealth.
* **The Deep Dive:** Untargeted poisoning raises loss or biases distribution. Targeted poisoning changes specific predictions. Backdoor examples correlate a trigger—token, pattern, style, image patch—with malicious output while normal behavior stays strong. Clean-label poisons look correctly labeled but alter features/boundaries. Availability can corrupt data or create resource-heavy inputs. Threat model attacker data access and influence.
* **Production Reality & Tradeoffs:** Detection uses provenance, influence/anomaly analysis, trigger search, clean validation, and behavior tests but none is complete. Robust training can hurt rare legitimate data.
A backdoor preserves normal utility while a trigger causes targeted behavior; clean-label attacks keep apparent labels correct while shaping representation; availability attacks broadly degrade service or training. Threat-model attacker access, poison budget, trigger control, and target. Evaluate clean accuracy and trigger success over transformations, paraphrases, modalities, and checkpoints. Simple outlier removal misses poisons designed to resemble the clean distribution; provenance and constrained contribution limits are essential.

* **Red Flag:** Calling ordinary noisy labels a backdoor.
