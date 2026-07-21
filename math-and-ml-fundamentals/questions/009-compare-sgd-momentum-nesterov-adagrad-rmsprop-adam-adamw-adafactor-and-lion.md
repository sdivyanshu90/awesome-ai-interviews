### Q: Compare SGD, momentum, Nesterov, AdaGrad, RMSProp, Adam, AdamW, Adafactor, and Lion.
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** These optimizers differ in momentum, coordinate-wise preconditioning, state memory, and regularization semantics. AdamW is a strong default for Transformers; SGD often gives strong vision generalization; Adafactor reduces state; Lion uses sign-like momentum updates—but schedule and tuning dominate labels.
* **The Deep Dive:** With gradient $g_t$, momentum uses $v_t=\mu v_{t-1}+g_t$ and $\theta_{t+1}=\theta_t-\eta v_t$; Nesterov evaluates or interprets the gradient at a look-ahead point. AdaGrad accumulates $v_t=v_{t-1}+g_t^2$, so learning rates only shrink and work well for sparse features. RMSProp uses an EMA $v_t=\beta_2v_{t-1}+(1-\beta_2)g_t^2$. Adam adds first moment $m_t$, bias corrections $\hat m_t,\hat v_t$, and updates

$$
\theta_{t+1}=\theta_t-\eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}.
$$

AdamW separately applies $\theta\leftarrow(1-\eta\lambda)\theta$, rather than putting $\lambda\theta$ through adaptive moments. Adafactor factorizes second-moment matrices into row/column statistics to cut $O(nm)$ state to $O(n+m)$; relative-step variants change tuning. Lion maintains momentum but updates primarily by its sign, saving one state tensor. The modern frontier after AdamW is matrix-aware preconditioning: Shampoo/SOAP precondition with Kronecker-factored second-moment statistics, and Muon orthogonalizes the momentum matrix via a few Newton–Schulz iterations, now used in frontier-scale pretraining. These pay off on 2D weight matrices, where whitening the update spectrum beats coordinate-wise scaling; embeddings, norms, and other 1D parameters typically stay on AdamW.

| Optimizer | Per-parameter state | Main behavior |
|---|---:|---|
| SGD | 0 | raw noisy gradient |
| Momentum/Lion | 1 | temporal smoothing/sign |
| Adam(W) | 2 | momentum + adaptive scale |
| Adafactor | factored | memory-efficient scale |
* **Production Reality & Tradeoffs:** Epsilon placement, precision, clipping, weight-decay exclusions, sharding, and schedule alter results. Adaptive methods can make tiny effective steps on stale large second moments. Compare update-to-weight ratios, loss, validation, and throughput at matched compute; optimizer memory is material at scale.
* **Red Flag:** Claiming one optimizer always generalizes better, or saying AdamW and Adam plus L2 are equivalent.

