### Q: Distinguish aleatoric, epistemic, distributional, and model uncertainty in deployed systems.
* **Difficulty:** Principal
* **Category:** ML Systems
* **The 10-Second Pitch:** Aleatoric uncertainty is conditional outcome noise; epistemic is uncertainty about learned parameters/functions; distributional uncertainty is unfamiliar input/environment; model uncertainty includes misspecification. They require different detection and mitigation.
* **The Deep Dive:** In probabilistic regression $y=f_\theta(x)+\epsilon(x)$, predicted noise variance models heteroscedastic aleatoric uncertainty and does not vanish with more identical data. Epistemic uncertainty reflects limited observations about $f$ and can shrink with informative coverage; Bayesian posterior variance, deep ensembles, and bootstraps approximate it with different assumptions. Distributional uncertainty asks whether deployment $p_{deploy}(x,y)$ differs from training—covariate, label-prior, concept, temporal, or adversarial shift. Model uncertainty includes wrong hypothesis class, missing variables, objective mismatch, and approximate-inference error; more IID data may not fix it.

```text
same x, intrinsically variable y -> aleatoric
few nearby training examples       -> epistemic
x from a new population/time       -> distributional
wrong causal/features/objective    -> model misspecification
```

Validate uncertainty through calibration and selective risk: as the system abstains on higher uncertainty, accepted error should fall. Use controlled shift suites and outcome monitoring; softmax entropy alone conflates ambiguity with confidence artifacts. Route uncertain high-impact cases to evidence gathering/human review, not merely a different threshold.
* **Production Reality & Tradeoffs:** Ensembles are costly and often correlated; OOD detectors fail on novel shifts; likelihood can be high for OOD inputs. Labels are delayed/selective. Expose uncertainty only with a calibrated user action, and monitor subgroup coverage to avoid unequal abstention.
* **Red Flag:** Calling any low confidence epistemic uncertainty or claiming one scalar cleanly decomposes all four sources.

