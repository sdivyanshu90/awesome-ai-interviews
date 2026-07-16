### Q: Compare MHA, MQA, GQA, and latent/compressed attention in quality and KV-cache cost.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** They keep query-head expressiveness but reduce stored K/V: MHA has one KV head per query head, GQA shares KV within groups, MQA shares one globally, and latent attention stores a learned compressed state. Cache falls linearly with KV representation size.
* **The Deep Dive:** For $H_q$ query heads and $H_{kv}$ KV heads, group size is $g=H_q/H_{kv}$. Queries have $[B,H_q,T,d_h]$; K/V have $[B,H_{kv},T,d_h]$ and are broadcast/indexed across query groups. Cache bytes are $2LBTH_{kv}d_hs$, so GQA with $H_{kv}=H_q/8$ uses one-eighth MHA KV; MQA uses $1/H_q$. Query projection/attention outputs remain multi-head.

MHA lets every head learn independent addressing and content, maximizing flexibility. MQA can bottleneck K/V diversity and quality, especially when converting a pretrained MHA model without adaptation. GQA is the common compromise and can be created by pooling/checkpoint surgery followed by continued training. Latent/compressed attention projects K/V into a lower-dimensional latent cache and reconstructs or absorbs projections during attention; savings depend on latent width and positional-encoding compatibility, with more complex kernels.

```text
MHA: q0->k0/v0 q1->k1/v1 q2->k2/v2 q3->k3/v3
GQA: q0,q1->k0/v0   q2,q3->k1/v1
MQA: q0,q1,q2,q3 -> one k/v
```
* **Production Reality & Tradeoffs:** Fewer KV heads improve concurrency and bandwidth but do not reduce every projection/attention FLOP equally. Kernel support, tensor-parallel grouping, RoPE, quantization, and cache layout determine realized gains. Evaluate long context, retrieval, and decoding quality.
* **Red Flag:** Saying GQA reduces query heads or that KV savings equal total model-memory savings.

