### Q: Compare k-fold, stratified, grouped, and nested cross-validation, and explain how repeated model selection overfits the validation set.
* **Difficulty:** Mid
* **Category:** Statistics
* **The 10-Second Pitch:** k-fold rotates a held-out fold to estimate how the training *procedure* generalizes; stratification preserves label proportions per fold, grouping keeps correlated samples (same patient, user, session) in one fold, and nesting separates hyperparameter search from performance reporting. The maximum of $T$ noisy search scores is optimistically biased by roughly $\sigma\sqrt{2\ln T}$; nested CV removes that bias from the reported number.
* **The Deep Dive:** Plain k-fold partitions $n$ samples into $K$ disjoint folds, trains on $K-1$ folds, scores on the held-out fold, and averages the $K$ scores; the result estimates the expected generalization error of the training procedure applied to about $n(K-1)/K$ samples, not the error of any single fitted model. Small $K$ is pessimistically biased (each fit sees less data); leave-one-out has minimal bias but high variance and $n$ times the cost. The variants exist to preserve one invariant: the fold structure must reproduce the independence structure that will hold at deployment. Stratified k-fold enforces per-fold class proportions, which matters for imbalanced or small datasets where a random fold can contain zero positives. Grouped k-fold assigns every sample of an entity to the same fold; if a patient's scans span the train/validation boundary, the model memorizes the patient and the score reflects leakage, not skill. Time-ordered data additionally requires forward-chaining splits so validation is always in the future of training.

  Repeated model selection breaks even a perfectly constructed split. Suppose $T$ configurations all have the same true score $v^{\star}$ and each measured score is $\hat v_t = v^{\star} + \varepsilon_t$ with independent noise of standard deviation $\sigma$. Selecting the argmax then reports

  $$
  \mathbb{E}\!\left[\max_{t\le T}\hat v_t\right]-v^{\star}\;\approx\;\sigma\sqrt{2\ln T},
  \qquad
  \sigma=\sqrt{\frac{p(1-p)}{n_{\text{val}}}}
  $$

  for an accuracy metric with true accuracy $p$. Worked example: $n_{\text{val}}=2000$, $p=0.8$ gives $\sigma=\sqrt{0.16/2000}\approx 0.0089$; a random search over $T=100$ configurations inflates the winner's score by about $0.0089\times\sqrt{2\ln 100}\approx 0.0089\times 3.03\approx 0.027$ — you report 82.7% for a model whose true accuracy is 80%. Bayesian optimization is worse per query because it deliberately concentrates trials where noise pushed earlier estimates up, and successive halving promotes configurations that got lucky on small early budgets. The bias shrinks as differences between configurations grow relative to $\sigma$; the limiting case is the falsifiable test: on randomly permuted labels every configuration has identical true accuracy $1/C$, yet tuned k-fold will report visibly above chance, while a nested outer estimate stays at chance.

  ```text
  outer fold k (never touched by any selection decision):
    [ trainval ------------------------------ ][ outer test ]
          |                                          ^
          inner CV over trainval only:               |
            split -> score T configs -> pick best    |
            refit best config on all trainval -------+
  repeat k = 1..K_outer; report mean/std of outer-test scores
  inner scores drive selection; outer scores drive the report — never mix
  ```

  Nested cross-validation is the fix shown above: the inner loop performs the entire search (including preprocessing choices and early stopping), the winner is refit on the full inner data, and the outer fold — statistically untouched by selection — scores it. The outer average is an approximately unbiased estimate of the *pipeline* "search, select, refit," which is what you will actually deploy. Nested CV does not name one winning setting; you rerun the search on all data and quote the outer estimate as its score.
* **Production Reality & Tradeoffs:** Nested CV costs roughly $K_{\text{outer}}(K_{\text{inner}}T+1)$ fits, so for deep models teams substitute a single train/validation split plus a locked test set evaluated once — the same statistical contract at lower cost. Grouped splitting requires entity keys to survive the feature pipeline; losing them silently reintroduces leakage. Stratifying regression targets needs quantile binning. Repeated k-fold reduces the variance of the estimate but does nothing about selection bias, and in production, distribution shift between the CV data and live traffic usually dominates fold-level noise anyway.
* **Red Flag:** Reporting the best score found during hyperparameter search as the expected deployed performance, or claiming k-fold "prevents leakage" while rows from the same user appear in both training and validation folds — the split type, not the fold count, is what controls leakage.
