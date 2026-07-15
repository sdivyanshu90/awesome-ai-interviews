### Q: How do batch size, gradient accumulation, learning-rate scaling, and gradient-noise scale interact?
* **Difficulty:** Principal
* **Category:** Optimization
* **The 10-Second Pitch:** A minibatch gradient is a noisy estimator whose variance falls roughly as $1/B$ only under weak dependence. Accumulation reproduces a larger statistical batch when weights and stochastic state remain fixed. Learning-rate scaling is regime-dependent, and beyond the critical batch extra samples improve hardware throughput more than optimization.
* **The Deep Dive:** Let per-example gradients have mean $g$ and covariance $\Sigma$. For IID sampling,

$$
\operatorname{Cov}(\hat g_B)=\frac{\Sigma}{B}.
$$

Gradient accumulation over $K$ microbatches of size $b$ computes the same mean as batch $B=Kb$ only if parameters are not updated between microbatches and loss reduction, masks, dropout/RNG, BatchNorm, clipping, and distributed synchronization have equivalent semantics. Summing rather than averaging silently multiplies the effective learning rate.

In a noise-dominated regime, linear learning-rate scaling $\eta\propto B$ can preserve update noise per processed example, often with warmup; square-root scaling arises under other stochastic approximations. Neither is universal. A useful gradient-noise-scale estimate compares variance to squared mean and identifies a critical batch: below it, larger batches reduce steps-to-quality; above it, gradient estimates are already accurate and larger batches mostly reduce the number of updates, risking worse time-to-quality unless training tokens or schedule change.

```text
small B: noisy updates, many optimizer steps, weak hardware use
       -> increase B -> better utilization and fewer noisy steps
critical B
       -> increase B -> diminishing optimization return, fewer update opportunities
```

Compare runs by tokens/examples processed and wall time, not epochs alone.
* **Production Reality & Tradeoffs:** Large batches consume activation memory, increase collective volume, reduce stochastic regularization, and may require optimizer/schedule changes. Accumulation saves activation memory but not optimizer state and increases time between updates. Data duplication, sequence packing, and token-count variation invalidate simple example-based scaling.
* **Red Flag:** Applying linear scaling mechanically, or claiming accumulation is identical while clipping each microbatch or updating BatchNorm statistics differently.

