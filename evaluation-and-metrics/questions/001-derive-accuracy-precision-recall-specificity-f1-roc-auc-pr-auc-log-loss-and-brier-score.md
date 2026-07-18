### Q: Derive accuracy, precision, recall, specificity, F1, ROC-AUC, PR-AUC, log loss, and Brier score.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** Start from the confusion matrix: thresholded metrics measure different error costs, ranking metrics integrate over thresholds, and proper scoring rules evaluate the probabilities themselves.
* **The Deep Dive:** For binary labels, define TP, FP, TN, and FN. Then

$$
\operatorname{Precision}=\frac{TP}{TP+FP},\quad
\operatorname{Recall}=\frac{TP}{TP+FN},\quad
\operatorname{Specificity}=\frac{TN}{TN+FP},\quad
F_1=\frac{2TP}{2TP+FP+FN}.
$$

Accuracy is $(TP+TN)/N$. Undefined denominators require an explicit convention, not silent zeroing. ROC plots TPR against $FPR=FP/(FP+TN)$; ROC-AUC is the probability a random positive scores above a random negative, with half-credit for ties. PR’s baseline depends on prevalence, so name the integration convention. Binary log loss is $-[y\log p+(1-y)\log(1-p)]$ and Brier is $(p-y)^2$. Both are proper scoring rules, but log loss punishes confident errors more strongly.

```text
                 predicted +   predicted -
actual +              TP             FN
actual -              FP             TN
```

Metrics need sample weights and confidence intervals when observations are clustered or rare.
* **Production Reality & Tradeoffs:** Choose metrics from decision costs and prevalence. Accuracy can hide minority failure; F1 ignores TN and probability quality; AUC can look good where no operating threshold is usable. Report the confusion matrix at production thresholds plus calibration and slice metrics.
* **Red Flag:** Reciting formulas without defining the positive class, threshold, prevalence, or zero-denominator behavior.
