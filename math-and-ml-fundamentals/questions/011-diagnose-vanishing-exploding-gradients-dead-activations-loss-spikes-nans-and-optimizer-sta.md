### Q: Diagnose vanishing/exploding gradients, dead activations, loss spikes, NaNs, and optimizer-state corruption.
* **Difficulty:** Senior
* **Category:** Debugging
* **The 10-Second Pitch:** Instrument the training step as a causal chain—inputs, forward activations, loss terms, backward gradients, optimizer state, and parameter updates—and locate the first non-finite or abnormal layer/rank before changing hyperparameters.
* **The Deep Dive:** For a depth-$L$ composition, gradients contain products $J_1^TJ_2^T\cdots J_L^T$. Singular values persistently below one shrink directions; above one amplify them. Saturated sigmoid/tanh derivatives and dead ReLUs cause vanishing; large residual/attention logits, poor initialization, unstable normalization, or excessive learning rate cause explosion. Track per layer/rank: activation mean/std/max and non-finite count; gradient norm, max, zero fraction; parameter norm; update norm and $\|\Delta\theta\|/\|\theta\|$; optimizer first/second moments; loss components; scaler overflows; and data identifiers.

```text
batch -> forward layer 1 -> ... -> loss -> backward last -> ... -> first -> optimizer
          ^ first bad activation              ^ first bad gradient       ^ bad moment/update
```

Reproduce with the exact batch, seed, model/optimizer/scaler state, and rank. Disable fused kernels or use anomaly hooks only to localize. Bisect layers and loss terms. If the forward pass is finite but gradients first become NaN at a normalization or exponential, inspect denominator epsilon/logit scale. If gradients are finite but parameters become NaN, inspect unscale-before-clip, optimizer epsilon, weight decay, and corrupted moments.
* **Production Reality & Tradeoffs:** Global gradient clipping limits explosion but can hide the source and suppress every layer. Lower precision exposes overflow; FP32 reductions and stable primitives help. Distributed training may show the failure on one rank before collectives spread it. Recovery requires a known-good checkpoint including optimizer/scaler/RNG/data cursor, not weights alone.
* **Red Flag:** Reducing the learning rate immediately without finding the first bad tensor, or logging only the global gradient norm after it is already NaN.

