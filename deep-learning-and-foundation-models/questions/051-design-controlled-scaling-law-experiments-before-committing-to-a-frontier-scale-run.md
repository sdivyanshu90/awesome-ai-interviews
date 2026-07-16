### Q: Design controlled scaling-law experiments before committing to a frontier-scale run.
* **Difficulty:** Principal
* **Category:** Experimentation
* **The 10-Second Pitch:** Train a matrix of model sizes and token budgets with fixed architecture/data/optimizer definitions, use multiple compute-matched points and seeds, fit loss/capability curves with uncertainty, validate extrapolation on held-out scales, then choose the frontier run from quality, risk, and lifecycle cost.
* **The Deep Dive:** Freeze tokenizer, architecture family, data snapshot/mixture/quality, sequence length, optimizer/schedule policy, precision, and loss measurement. Select logarithmically spaced parameter sizes and, for each compute budget, several token allocations around the expected optimum—isoFLOP curves. Avoid one run per size, which confounds parameters and data. Measure training/validation NLL on uncontaminated fixed domains, downstream tasks, safety/robustness, throughput/utilization, and failure rate.

Fit models such as $L=L_\infty+aN^{-\alpha}+bD^{-\beta}$ using uncertainty-aware regression; inspect residuals and exclude warmup/transient regions by prespecified rules. Hold out the largest pilot scale to test extrapolation, bootstrap seeds/data slices, and run sensitivity to data quality/repetition and context. Use predictions to optimize not only training FLOPs but inference volume, latency, GPU availability, and data limits.

```text
size N × tokens D grid -> measured isoFLOP envelope -> fit + uncertainty -> held-out scale check -> frontier decision
```
* **Production Reality & Tradeoffs:** Regime changes in parallelism, batch, architecture, data, or optimizer invalidate smooth extrapolation. Capabilities/safety may not follow NLL. Budget pilot redundancy and checkpoint recovery; a failed frontier run is costlier than extra pilots.
* **Red Flag:** Fitting a power law through three confounded points and presenting a single extrapolated number without intervals or held-out validation.

