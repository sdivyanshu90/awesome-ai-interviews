### Q: Evaluate embedding anisotropy, hubness, dimensionality, multilingual alignment, and drift.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Inspect geometry and retrieval consequences: norm/mean/covariance spectrum and cosine distribution for anisotropy, neighbor occurrence for hubs, intrinsic/effective rank, cross-language paired retrieval, and anchored neighbor/rank changes across versions.
* **The Deep Dive:** Anisotropy means vectors occupy a narrow cone: measure mean vector norm, pairwise cosine baseline, covariance eigen spectrum/effective rank, and centered/whitened variants. Hubness means a few items appear in many k-NN lists; compute k-occurrence/skew, source/domain/length of hubs, and query-conditioned impact. High nominal dimension can have low intrinsic/effective dimension; participation ratio $(\sum\lambda_i)^2/\sum\lambda_i^2$ is one diagnostic.

Multilingual alignment uses translation pairs and cross-language retrieval, nearest-neighbor consistency, and fertility/domain/language slices; aggregate cosine is insufficient. Drift compares same anchored documents/queries across model/preprocessing/index versions: norm/spectrum, pairwise similarity correlation, nearest-neighbor overlap, rank changes, retrieval metrics, and task outcomes. Separate legitimate new geometry from online/offline mismatch.

Counterfactuals vary document length/template to find nonsemantic norm/hub artifacts. Any whitening/centering changes the deployed metric and must be trained/versioned/evaluated.
* **Production Reality & Tradeoffs:** Geometry diagnostics do not prove relevance. ANN/quantization can create apparent drift. Multilingual pairs may be culturally non-equivalent. Store privacy-safe anchors and always pair with judged retrieval.
* **Red Flag:** Declaring collapse from one t-SNE plot or fixing anisotropy by centering without downstream evaluation.

