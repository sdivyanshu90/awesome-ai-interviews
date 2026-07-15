### Q: Explain trees, random forests, gradient boosting, XGBoost-style objectives, and feature importance traps.
* **Difficulty:** Senior
* **Category:** Classical ML
* **The 10-Second Pitch:** Trees greedily partition space; forests average decorrelated trees to reduce variance; boosting adds weak trees along loss gradients to reduce bias. XGBoost-style split gains use first/second-order statistics plus regularization. Common feature importances are biased or noncausal.
* **The Deep Dive:** A CART tree chooses feature/threshold maximizing impurity or loss reduction, recursively creating piecewise-constant regions. Deep trees have low bias/high variance. Random forests train bootstrapped trees with random feature subsets; averaging reduces variance when errors are not perfectly correlated. Gradient boosting builds $F_t=F_{t-1}+\eta f_t$ where $f_t$ fits negative functional gradients.

With gradient $g_i$ and Hessian $h_i$, a regularized leaf has optimal weight $w^*=-G/(H+\lambda)$. A split gain compares left/right versus parent:

$$
\frac12\left[\frac{G_L^2}{H_L+\lambda}+\frac{G_R^2}{H_R+\lambda}-\frac{G^2}{H+\lambda}\right]-\gamma.
$$

Control depth/leaves, minimum child weight, row/column sampling, learning rate, and early stopping. Impurity/gain importance favors high-cardinality/continuous features and distributes credit among correlated features. Permutation importance breaks dependencies and depends on evaluation distribution. SHAP explains model predictions under a background/conditional assumption; none establishes causality.
* **Production Reality & Tradeoffs:** Boosted trees are excellent for tabular data but can overfit leakage and require monotonic/interaction constraints in regulated use. Missing-value routing is learned and can encode collection artifacts. Validate temporally and inspect stability across seeds/slices.
* **Red Flag:** Using built-in feature importance as proof a feature causes the outcome.

