### Q: Compare generative and discriminative classifiers, including Naive Bayes and logistic regression.
* **Difficulty:** Senior
* **Category:** Classical ML
* **The 10-Second Pitch:** Generative classifiers model $p(x\mid y)p(y)$ and derive $p(y\mid x)$; discriminative classifiers model the posterior or boundary directly. Generative assumptions can improve sample efficiency and missing-data handling, while discriminative models usually win asymptotically when their conditional form is suitable.
* **The Deep Dive:** Naive Bayes assumes feature conditional independence:

$$
p(y\mid x)\propto p(y)\prod_jp(x_j\mid y).
$$

Despite unrealistic independence, its log posterior is additive and often strong for sparse text. Gaussian class-conditionals with shared covariance induce a linear discriminant; class-specific covariance gives quadratic boundaries. Logistic regression directly sets $\log[p(y=1\mid x)/(1-p)]=w^Tx+b$ and maximizes conditional likelihood without modeling $p(x)$.

A correctly specified generative model can estimate parameters efficiently from less data and naturally generate/impute under its joint assumptions. A discriminative model spends capacity only on the boundary and is robust to misspecification of irrelevant $p(x)$. With enough labeled data it often yields lower classification error. Semi-supervised learning helps a generative model only when unlabeled-data assumptions align; a bad marginal model can hurt. Calibration depends on regularization, priors, and shift for both.
* **Production Reality & Tradeoffs:** Naive Bayes probabilities are often overconfident due to correlated features; smooth zero counts. Logistic regression needs representative labels and feature engineering for nonlinear boundaries. Compare log loss/calibration and threshold utility, not accuracy alone.
* **Red Flag:** Saying generative means neural text generation, or that modeling $p(x)$ automatically improves classification.

