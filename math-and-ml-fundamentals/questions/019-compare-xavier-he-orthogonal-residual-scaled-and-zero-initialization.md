### Q: Compare Xavier, He, orthogonal, residual-scaled, and zero initialization.
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** Initialization controls forward activation and backward gradient variance before learning. Xavier suits approximately symmetric activations; He compensates for ReLU gating; orthogonal preserves norms in linear directions; residual scaling keeps accumulated branches stable; indiscriminate zero initialization destroys symmetry.
* **The Deep Dive:** For $y_j=\sum_{i=1}^{fan_{in}}w_{ji}x_i$ with independent zero-mean terms, $\operatorname{Var}(y_j)\approx fan_{in}\operatorname{Var}(w)\operatorname{Var}(x)$. Xavier balances forward and backward variance with roughly $\operatorname{Var}(w)=2/(fan_{in}+fan_{out})$ (or $1/fan_{in}$ under a simpler derivation). ReLU zeros about half of symmetric inputs, motivating He variance $2/fan_{in}$. The gain changes for leaky ReLU, tanh, and other activations.

Orthogonal matrices have singular values near one before nonlinearities and can help deep/recurrent signal propagation; scale by the activation gain. In residual stacks, summing many branches can grow variance, so scale residual weights by depth, initialize the final residual projection small/zero, or use architecture-specific schemes such as Fixup. Zeroing every weight makes neurons receive identical gradients, so symmetry never breaks. Zeroing only a final residual/adapter factor is useful because another nonzero factor/path carries gradients. Width-scaling parameterizations such as muP set initialization variance and per-layer learning rates jointly so optimal hyperparameters transfer across widths, letting small proxy models tune large ones.

Check empirical activation, residual, attention-logit, and gradient distributions on real batches before training.
* **Production Reality & Tradeoffs:** Independence assumptions fail with attention, convolutions, normalization, gating, tying, and residuals. Framework fan conventions and truncated-normal variance matter. Initialization cannot rescue an unstable learning rate or loss scale, but bad initialization can make early optimization irrecoverable.
* **Red Flag:** Using Xavier or He by name without matching activation/fan convention, or claiming all zero initialization is safe because gradients exist.

