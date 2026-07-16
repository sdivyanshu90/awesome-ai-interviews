### Q: Compare curriculum learning, data annealing, continued pretraining, and domain-adaptive pretraining.
* **Difficulty:** Senior
* **Category:** Training
* **The 10-Second Pitch:** Curriculum orders examples by difficulty/structure; data annealing changes mixture over training; continued pretraining resumes a base model on new general data; domain-adaptive pretraining concentrates on a target domain. All alter optimization history and forgetting risk.
* **The Deep Dive:** A curriculum schedules a difficulty measure—length, noise, reasoning steps, resolution—so early data changes the basin/skills available later. It helps only when difficulty is meaningful and avoids permanently starving hard examples. Data annealing continuously changes source weights, often broad early and high-quality/domain-specific late; token totals and effective repeats per source matter. Continued pretraining (CPT) resumes next-token/objective training with tokenizer/model usually fixed, to add fresher or more data. Domain-adaptive pretraining (DAPT) is CPT on domain unlabeled text before supervised/preference tuning; task-adaptive pretraining narrows further to task-like text.

Track target validation plus general/multilingual/safety retention throughout. Mix replay/general data, lower LR, adapters, regularization, or shorter schedules to reduce forgetting. Reset or retain optimizer state deliberately: old moments may mismatch new gradients, while reset changes effective warmup. Deduplicate downstream eval from CPT data.
* **Production Reality & Tradeoffs:** Late annealed data has disproportionate influence; narrow DAPT can degrade generality/calibration and repeat sensitive text. Curriculum adds pipeline complexity and may merely delay necessary hard learning. Compare at matched tokens/compute.
* **Red Flag:** Calling any chronological data ordering a curriculum, or evaluating DAPT only on its target domain.

