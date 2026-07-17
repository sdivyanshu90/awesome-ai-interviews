### Q: Design preference collection with pairwise labels, rankings, ties, disagreement, and annotator calibration.
* **Difficulty:** Senior
* **Category:** Data
* **The 10-Second Pitch:** Define atomic rubrics and collect comparisons under blinded randomized presentation, allowing ties/invalid cases. Preserve annotator identity/confidence and disagreement rather than forcing a false total order.
* **The Deep Dive:** Sample prompts by production risk and hard model disagreements. Show response pairs with position randomized and matched truncation. Separate correctness, helpfulness, safety, style, and tool behavior; otherwise annotators trade objectives inconsistently. Rankings can yield multiple pairs but correlated labels must be handled. Gold/calibration tasks, duplicate items, adjudication, and inter-rater analysis detect quality. Model annotator preference with uncertainty or hierarchical effects.
* **Production Reality & Tradeoffs:** Pairwise labels are easier than absolute scores but still biased by verbosity, style, and cultural assumptions. Protect sensitive prompts and compensate domain experts. Version instructions and UI.
Sampling policy matters as much as the label UI. Mix representative traffic, disagreement-heavy candidates, and safety-critical strata with logged selection probabilities. Keep prompt groups intact across splits. Measure inter- and intra-annotator consistency without erasing legitimate plural preferences; adjudicate policy questions separately from subjective style. A reward model trained on forced choices between two bad answers learns the wrong boundary.

* **Red Flag:** Using majority vote while discarding systematic subgroup disagreement.
