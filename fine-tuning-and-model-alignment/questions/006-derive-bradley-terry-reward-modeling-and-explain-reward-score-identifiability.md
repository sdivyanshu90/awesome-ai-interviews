### Q: Derive Bradley–Terry reward modeling and explain reward-score identifiability.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Given prompt $x$ and preferred response $y_w$ over $y_l$, train a scalar reward $r_\phi$ with the Bradley-Terry loss $-\log\sigma(r_\phi(x,y_w)-r_\phi(x,y_l))$. The model learns relative preference, not an absolute measure of goodness.
* **The Deep Dive:** Annotators produce comparisons, rankings, or scalar labels; pairwise data is generally more reliable than unconstrained scalar scoring. The likelihood assumes preference probability is logistic in reward difference. Data must represent ambiguity, ties, annotator disagreement, and policy dimensions; otherwise reward scores inherit label bias.
Bradley–Terry is invariant to adding a constant to all rewards, so absolute reward is unidentifiable. Model ties with soft targets or a tie-aware likelihood; do not invent winners. Rankings yield dependent pairs and need weighting. Inspect cycles, annotator disagreement, prompt-slice calibration, and margin histograms. High pair accuracy does not prove extrapolation: an optimized policy may generate responses outside the comparison distribution where reward estimates fail.

* **Production Reality & Tradeoffs:** Evaluate reward models on held-out human comparisons and adversarial examples. A strong reward-model accuracy can still be exploitable outside its training distribution.
* **Red Flag:** Treating reward-model output as an objective truth score.
