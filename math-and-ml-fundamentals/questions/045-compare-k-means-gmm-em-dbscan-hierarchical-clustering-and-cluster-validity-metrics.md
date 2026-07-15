### Q: Compare k-means, GMM/EM, DBSCAN, hierarchical clustering, and cluster-validity metrics.
* **Difficulty:** Senior
* **Category:** Unsupervised Learning
* **The 10-Second Pitch:** k-means fits spherical hard clusters; GMM gives probabilistic ellipsoidal memberships via EM; DBSCAN finds density-connected shapes/noise; hierarchical clustering builds a linkage tree. Validity scores encode assumptions and need stability/domain checks.
* **The Deep Dive:** k-means minimizes $\sum_i\|x_i-\mu_{z_i}\|^2$ by alternating assignment and centroid updates; it is sensitive to scale, initialization, outliers, and $k$. A GMM maximizes $\sum_i\log\sum_k\pi_k\mathcal N(x_i;\mu_k,\Sigma_k)$; EM computes responsibilities then weighted parameter updates. Covariance regularization prevents singular components. DBSCAN uses radius $\epsilon$ and `min_samples` to connect dense core points, labels sparse points noise, and struggles with variable density/high dimension. Agglomerative clustering repeatedly merges clusters using single/complete/average/Ward linkage; the dendrogram exposes resolutions but early merges are irreversible.

Silhouette compares within-cluster cohesion to nearest-cluster separation; Davies–Bouldin and Calinski–Harabasz have geometric biases. GMM AIC/BIC compare likelihood with complexity. More important: bootstrap stability, perturbation consistency, held-out likelihood where meaningful, and downstream/domain interpretability.

| Method | Shape | Assignment | Needs $k$ |
|---|---|---|---|
| k-means | spherical | hard | yes |
| GMM | ellipsoidal | soft | yes |
| DBSCAN | density-connected | hard/noise | no |
| hierarchical | linkage-defined | tree | cut later |
* **Production Reality & Tradeoffs:** Embeddings and distance metric define the clusters more than the algorithm. Internal metrics can reward meaningless partitions. Do not assign semantic labels without external validation and subgroup analysis.
* **Red Flag:** Selecting $k$ solely from one elbow plot or treating clusters as natural ground-truth classes.

