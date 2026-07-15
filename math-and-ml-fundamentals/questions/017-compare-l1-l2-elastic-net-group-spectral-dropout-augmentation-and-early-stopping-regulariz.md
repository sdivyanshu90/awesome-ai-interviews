### Q: Compare L1, L2, elastic-net, group, spectral, dropout, augmentation, and early-stopping regularization.
* **Difficulty:** Senior
* **Category:** Conceptual
* **The 10-Second Pitch:** Regularizers encode different priors: sparsity, small norm, grouped selection, bounded operator gain, stochastic co-adaptation control, invariance from data, or limited optimization time. They are not interchangeable knobs for “less overfitting.”
* **The Deep Dive:** L2 adds $\lambda\|w\|_2^2/2$ and shrinks continuously; under linear-Gaussian assumptions it is a Gaussian-prior MAP objective. L1 adds $\lambda\|w\|_1$ and its kink can set coefficients exactly to zero. Elastic net combines both and stabilizes selection among correlated features. Group lasso penalizes $\sum_g\|w_g\|_2$, selecting/removing predefined groups. Spectral penalties or normalization constrain largest singular values and sensitivity rather than entrywise magnitude.

Dropout samples masks during training and rescales to preserve expectation; its ensemble interpretation is approximate and interactions with normalization/residual architectures matter. Data augmentation encodes invariance/equivariance only when transformations preserve labels; invalid transforms introduce bias. Early stopping limits movement along slowly learned/ill-conditioned directions and acts like optimization-dependent regularization. Label smoothing and mixup alter targets/data geometry, while weight decay alters parameters.

Ablate at matched optimization budgets and report train objective, validation target metric, calibration, robustness, and subgroup effects. Combining regularizers can overconstrain the same behavior.
* **Production Reality & Tradeoffs:** AdamW decay is not identical to adding L2 to Adam’s gradient. L1 sparsity may not produce hardware speed without structured kernels. Augmentation adds compute and can shift semantics. Early stopping requires a representative validation metric and patience robust to noise.
* **Red Flag:** Listing regularizers without stating the prior/invariance they impose, or applying augmentations that do not preserve the task label.

