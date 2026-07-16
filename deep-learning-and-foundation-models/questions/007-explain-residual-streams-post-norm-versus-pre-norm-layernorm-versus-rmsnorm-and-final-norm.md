### Q: Explain residual streams, post-norm versus pre-norm, LayerNorm versus RMSNorm, and final normalization.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** The residual stream is the model’s persistent width-$D$ state; attention and MLP blocks write updates into it. Pre-norm gives an identity gradient path, post-norm normalizes after addition, RMSNorm omits mean subtraction, and a final norm stabilizes logits.
* **The Deep Dive:** A pre-norm block is $x'=x+\operatorname{Attn}(N(x))$, $y=x'+\operatorname{MLP}(N(x'))$. Its Jacobian contains an explicit identity plus branch term, helping very deep optimization. Original post-norm uses $x'=N(x+\operatorname{Attn}(x))$; it controls each output scale but gradients repeatedly pass through normalization and often need careful warmup/scaling. Sandwich/hybrid variants change this trade-off.

LayerNorm computes $(x-\mu)/\sqrt{\sigma^2+\epsilon}$ over hidden features, then scale/bias. RMSNorm computes $x/\sqrt{E[x^2]+\epsilon}$ with scale, retaining the mean and saving operations. Normalization does not independently normalize each attention head unless axes say so. The MLP, commonly SwiGLU $W_d(\operatorname{SiLU}(W_gx)\odot W_ux)$, performs token-wise nonlinear feature transformation and holds much of the parameters; attention mixes tokens.

```text
residual x -> norm -> attention -> add -> residual x'
             x' -> norm -> MLP       -> add -> residual y -> ... -> final norm -> logits
```
* **Production Reality & Tradeoffs:** Residual magnitude can grow with depth even when branches are normalized; initialization, residual scaling, and final norm matter. Norms are memory-bandwidth-heavy and fused in serving. Epsilon/dtype and zero-variance tokens affect stability.
* **Red Flag:** Saying LayerNorm normalizes across the batch, or treating the residual connection as merely a skip used when shapes differ.

