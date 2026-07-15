### Q: How do calibration, thresholding, expected utility, and asymmetric error costs become a product decision?
* **Difficulty:** Principal
* **Category:** Statistics
* **The 10-Second Pitch:** Calibration concerns probability meaning; thresholding converts probabilities into actions under costs and constraints. Validate reliability on representative slices, then choose the action minimizing expected loss—not a universal 0.5 cutoff.
* **The Deep Dive:** A binary predictor is calibrated if $P(Y=1\mid \hat p=p)=p$. Inspect reliability diagrams with support, NLL, Brier score, and binning-sensitive ECE; for multiclass problems evaluate top-label and classwise calibration. AUC/ranking quality is distinct: a monotonic transformation preserves ranking while changing calibration.

For actions positive/negative with false-positive cost $C_{FP}$ and false-negative cost $C_{FN}$, calibrated probabilities and zero correct-action costs imply predict positive when

$$
C_{FP}(1-p)<C_{FN}p
\quad\Longleftrightarrow\quad
p>\frac{C_{FP}}{C_{FP}+C_{FN}}.
$$

Real products add intervention benefit, review capacity, fairness, abstention, and delayed effects, so select thresholds on a held-out, temporally representative set under those constraints. Plot expected utility and confusion counts versus threshold with confidence intervals. Temperature/Platt scaling fit a parametric map; isotonic is flexible but data-hungry; recalibration must use held-out data.

```text
score -> calibrated probability -> cost/capacity policy -> action / review / abstain
              ^                            ^
       reliability monitoring       threshold monitoring
```
* **Production Reality & Tradeoffs:** Calibration drifts with prevalence, population, upstream routing, and model changes. Global calibration can hide severe subgroup miscalibration. Class weighting/oversampling changes posterior interpretation. If labels arrive late or selectively, online calibration estimates are biased.
* **Red Flag:** Using 0.5 because it is the default, or choosing a threshold on the test set to maximize F1 without modeling production costs.

