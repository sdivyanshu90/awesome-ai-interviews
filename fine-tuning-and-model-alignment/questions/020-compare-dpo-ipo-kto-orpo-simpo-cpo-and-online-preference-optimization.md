### Q: Compare DPO, IPO, KTO, ORPO, SimPO, CPO, and online preference optimization.
* **Difficulty:** Principal
* **Category:** Alignment
* **The 10-Second Pitch:** These methods convert preference signals into different offline objectives or reference constraints. DPO uses reference-relative pairwise log odds; alternatives alter loss geometry, reference dependence, unpaired feedback, or combine SFT and preference terms.
* **The Deep Dive:** DPO applies logistic loss to chosen-minus-rejected policy log ratios relative to a reference. IPO uses a squared target/margin motivated by avoiding overfitting separable preferences. KTO can use desirable/undesirable examples without explicit pairs through prospect-theoretic utility. ORPO combines supervised likelihood with an odds-ratio preference penalty without a separate reference. SimPO uses length-normalized policy likelihood and a margin. CPO names vary across literature, so state the exact objective. Online methods collect preferences from current policies, reducing distribution mismatch.
* **Production Reality & Tradeoffs:** Names/implementations evolve; compare equations, masking, length normalization, beta/margin, and reference. Offline simplicity trades exploration and can overfit biased pairs.
* **Red Flag:** Listing acronyms without deriving how chosen and rejected probabilities are changed.

