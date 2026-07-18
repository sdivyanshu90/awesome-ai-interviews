### Q: Diagnose position, verbosity, style, self-preference, reference, contamination, and prompt biases in judges.
* **Difficulty:** Principal
* **Category:** LLM-as-a-Judge
* **The 10-Second Pitch:** Use counterbalanced transformations that should preserve quality and measure score/winner changes; bias is causal sensitivity to irrelevant presentation, identity, familiarity, or judge configuration.
* **The Deep Dive:** For pairwise position bias, evaluate AB and BA and report flip rate and first/second-position advantage. Match or deliberately vary length to estimate verbosity preference; paraphrase style, formatting, confidence, and politeness while preserving content. Blind model/provider identity and include judge-family outputs to test self-preference/familiarity. Evaluate with correct, missing, and subtly flawed references to expose reference anchoring. Search benchmark/judge training contamination through canaries, temporal splits, and novel private cases. Perturb rubric ordering, labels, examples, and prompt wording to quantify prompt sensitivity. Fit a regression or hierarchical model with quality labels and nuisance factors, but retain direct counterfactual rates. Adjudicate cases where humans and judge diverge.
* **Production Reality & Tradeoffs:** No debiasing prompt eliminates model bias. Swapping order doubles cost; multiple judges may share training data and correlated preferences. Maintain bias dashboards by task/language/length and gate judge upgrades with replay.
* **Red Flag:** Assuming temperature zero or chain-of-thought makes a judge objective.

