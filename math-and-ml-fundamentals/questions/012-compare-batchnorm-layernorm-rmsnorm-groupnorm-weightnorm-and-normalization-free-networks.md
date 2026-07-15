### Q: Compare BatchNorm, LayerNorm, RMSNorm, GroupNorm, WeightNorm, and normalization-free networks.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Normalization methods differ by reduction axes and whether they normalize activations, weights, or neither. BatchNorm uses batch statistics and running state; LayerNorm/RMSNorm are per token; GroupNorm is per sample/channel group; WeightNorm reparameterizes weights.
* **The Deep Dive:** For values $x_i$ over a chosen set $S$, normalization commonly computes

$$
\hat x_i=\frac{x_i-\mu_S}{\sqrt{\sigma_S^2+\epsilon}},\qquad y_i=\gamma_i\hat x_i+\beta_i.
$$

BatchNorm for NCHW reduces over batch and spatial axes per channel, uses minibatch statistics in training, and running estimates at inference. LayerNorm reduces over hidden features independently for each token/example, so train and inference semantics match. RMSNorm omits mean subtraction: $x/\sqrt{\frac1d\sum_i x_i^2+\epsilon}$, usually with learned scale; it is cheaper and preserves mean information. GroupNorm divides channels into groups and reduces within each sample, working at small batch. WeightNorm writes $w=g\,v/\|v\|$, decoupling direction and scale without normalizing activations. Normalization-free networks instead use initialization, residual scaling, activation control, and sometimes weight standardization.

| Method | Statistics depend on other examples? | Stateful inference? | Common use |
|---|---:|---:|---|
| BatchNorm | yes | yes | CNNs with stable batches |
| LayerNorm | no | no | Transformers |
| RMSNorm | no | no | LLMs |
| GroupNorm | no | no | small-batch vision |
* **Production Reality & Tradeoffs:** BatchNorm breaks under tiny/non-IID microbatches unless synchronized, which adds collectives. Epsilon placement and accumulation precision affect low-precision stability. Pre-norm Transformers optimize deeply but may change residual scaling; post-norm has different stability/representation behavior. Normalization can remove useful scale information and costs memory bandwidth.
* **Red Flag:** Saying all normalization reduces across the batch, or using BatchNorm running statistics incorrectly during evaluation.

