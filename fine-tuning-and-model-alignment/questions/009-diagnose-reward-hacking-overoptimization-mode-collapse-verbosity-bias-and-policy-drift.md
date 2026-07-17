### Q: Diagnose reward hacking, overoptimization, mode collapse, verbosity bias, and policy drift.
* **Difficulty:** Principal
* **Category:** Safety
* **The 10-Second Pitch:** Reward hacking occurs when the policy finds outputs that score well under a proxy reward while violating intended human preference. Control it with diverse preference data, held-out human evaluation, KL constraints, adversarial red teaming, reward ensembles, and early stopping on true utility.
* **The Deep Dive:** A reward model may reward verbosity, confident style, superficial citations, or refusal patterns rather than factual help. As policy optimization pushes farther from the reward model’s training distribution, proxy error grows. Monitor reward versus independently judged quality; divergence is a warning signal, not success.
Overoptimization creates a Goodhart curve: proxy reward rises after independent utility peaks. Compare checkpoints by KL distance using blinded humans and held-out judges. Slice verbosity, refusal, citation style, language, and rare tasks. Conservative KL, ensembles, uncertainty penalties, adversarial data, and early stopping reduce exploitation but do not replace checking whether the policy has left reward-model support.

* **Production Reality & Tradeoffs:** Reward ensembles and uncertainty gates increase cost but reduce single-model exploitation. Keep evaluation models and prompts separate from training signals to limit benchmark gaming.
* **Red Flag:** Assuming higher reward-model score necessarily means better aligned behavior.
