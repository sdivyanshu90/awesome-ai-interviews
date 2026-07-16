### Q: Derive multi-head attention including projection, reshape, transpose, concatenation, and $W_O$.
* **Difficulty:** Mid
* **Category:** Conceptual
* **The 10-Second Pitch:** Project the residual stream into head-specific Q/K/V subspaces, reshape to expose heads, run independent attention nonlinearities, concatenate head outputs, and mix them with $W_O$. Multiple softmax maps are not generally reducible to one wide head.
* **The Deep Dive:** For $X\in\mathbb R^{B\times T\times D}$ and $H$ heads with $d_h=D/H$, fused projections produce $Q_{flat}=XW_Q\in\mathbb R^{B\times T\times Hd_h}$ and similarly K/V. Reshape $[B,T,H,d_h]$ and transpose to $[B,H,T,d_h]$. Each head computes

$$
O_h=\operatorname{softmax}\left(\frac{Q_hK_h^T}{\sqrt{d_h}}+M\right)V_h.
$$

Transpose/concatenate $[B,H,T,d_h]\to[B,T,Hd_h]$ and output-project $Y=\operatorname{Concat}(O_1,\ldots,O_H)W_O$, $W_O\in\mathbb R^{Hd_h\times D}$. $W_O$ mixes information across heads and restores residual width.

```text
[B,T,D] -- WQ/WK/WV --> [B,T,H,d_h] -- transpose --> [B,H,T,d_h]
       per-head score/softmax/value mix -> [B,H,T,d_h]
       transpose + concatenate -> [B,T,H*d_h] -- WO --> [B,T,D]
```

A single wide head has one softmax distribution; multi-head attention has $H$ independently normalized distributions, so softmax nonlinearity prevents general equivalence. Head redundancy is empirical and can support pruning/GQA.
* **Production Reality & Tradeoffs:** Projection GEMMs dominate prefill while KV reads dominate decode. Head dimension must suit kernels/position encoding. Tensor parallelism often shards heads; incorrect reshape/transposes silently mix tokens/heads. GQA changes KV head count, not query head count.
* **Red Flag:** Saying heads merely split compute with identical attention, or omitting $W_O$ and tensor dimensions.

