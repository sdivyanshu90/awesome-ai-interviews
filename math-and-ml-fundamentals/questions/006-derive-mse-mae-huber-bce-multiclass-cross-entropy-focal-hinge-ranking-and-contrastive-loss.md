### Q: Derive MSE, MAE, Huber, BCE, multiclass cross-entropy, focal, hinge, ranking, and contrastive losses.
* **Difficulty:** Senior
* **Category:** Conceptual
* **The 10-Second Pitch:** Loss choice encodes a noise model and decision geometry. MSE/MAE/Huber govern regression residuals; BCE and multiclass CE are Bernoulli/categorical negative log-likelihoods; focal reweights easy examples; hinge/ranking enforce margins; contrastive objectives shape relative representation similarity.
* **The Deep Dive:** For residual $r=\hat y-y$, MSE $\frac12r^2$ has gradient $r$ and unbounded outlier influence; MAE $|r|$ has subgradient $\operatorname{sign}(r)$; Huber is quadratic for $|r|\le\delta$ and linear outside with continuous gradient. BCE from logits $z$ is stably $\max(z,0)-zy+\log(1+e^{-|z|})$, whose derivative is $\sigma(z)-y$. Multiclass CE is

$$
L=-\log\frac{e^{z_y}}{\sum_j e^{z_j}}=-z_y+\operatorname{LSE}(z),
\qquad \frac{\partial L}{\partial z}=p-e_y.
$$

Label smoothing replaces $e_y$ with $(1-\epsilon)e_y+\epsilon/K$. Focal loss $-(1-p_t)^\gamma\log p_t$ reduces easy-example gradients but changes calibration. Hinge $\max(0,1-yf(x))$ and pairwise ranking $\max(0,m-s^++s^-)$ enforce margins. InfoNCE uses a softmax over one positive and sampled negatives; temperature and the negative distribution determine gradient pressure.

| Loss | Implied center | Key failure |
|---|---|---|
| MSE | conditional mean | outliers dominate |
| MAE | conditional median | nondifferentiable kink |
| CE | class distribution | label noise/overconfidence |
| contrastive | relative geometry | false or weak negatives |
* **Production Reality & Tradeoffs:** Weights, reductions, masks, sampling, and class imbalance change the actual objective. Never apply softmax before a fused CE-with-logits kernel. Evaluate target metric and calibration because optimizing a surrogate need not optimize business utility. For detection/retrieval, mining strategy can dominate nominal loss choice.
* **Red Flag:** Choosing losses from habit, claiming focal loss fixes imbalance by itself, or using probability-space BCE that underflows.

