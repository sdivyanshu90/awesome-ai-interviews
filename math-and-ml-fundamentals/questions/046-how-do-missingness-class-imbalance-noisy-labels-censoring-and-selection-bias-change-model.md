### Q: How do missingness, class imbalance, noisy labels, censoring, and selection bias change model design?
* **Difficulty:** Principal
* **Category:** ML Systems
* **The 10-Second Pitch:** These are data-generating and observation mechanisms, not preprocessing nuisances. Model what is observed, why it is missing/labeled/selected, and align training/evaluation with deployment decisions using weighting, robust objectives, survival methods, and sensitivity analysis.
* **The Deep Dive:** Missing completely at random, at random conditional on observed variables, and not at random imply different identifiability. Impute within folds, add missingness indicators when informative, and avoid treating sentinel values as ordinary. Class imbalance affects decision thresholds and estimator variance; use stratified sampling, calibrated costs/weights, appropriate PR/recall metrics, and enough rare positives—not synthetic balance alone.

Label noise may be symmetric, class-dependent, or instance-dependent. Audit annotator/source disagreement, use robust losses or probabilistic labels cautiously, and keep a trusted adjudicated set. Right censoring means event time is only known to exceed a bound; survival likelihoods and time-dependent metrics use that information, while dropping censored rows biases results. Selection bias occurs when labeled/training cases differ from deployment. Under covariate shift, importance weighting $p_{target}(x)/p_{train}(x)$ may correct expectations but high weights explode variance; concept/selection on unobservables needs assumptions or sensitivity bounds.

Draw the causal timing of features, selection, labels, and intervention before choosing a fix.
* **Production Reality & Tradeoffs:** Reweighting lowers effective sample size; resampling distorts probabilities; imputation understates uncertainty; weak noise correction can amplify errors. Report performance by missingness/selection mechanism and monitor changes after product interventions alter who gets labeled.
* **Red Flag:** Fixing imbalance by oversampling and reporting accuracy, or deleting censored/missing cases as “bad data.”

