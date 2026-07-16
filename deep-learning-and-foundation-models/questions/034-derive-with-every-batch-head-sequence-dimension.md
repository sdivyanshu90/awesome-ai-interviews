### Q: Derive $Q=XW_Q$, $K=XW_K$, $V=XW_V$ with every batch/head/sequence dimension.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** Project $X[B,T,D]$ to flattened head channels, reshape and transpose to $Q[B,H_q,T,d_k]$, $K[B,H_{kv},S,d_k]$, $V[B,H_{kv},S,d_v]$. Scores are $[B,H_q,T,S]$ after KV-head grouping/broadcast.
* **The Deep Dive:** For self-attention $S=T$ and $X_q=X_{kv}=X$. Dense matrices can be $W_Q[D,H_qd_k]$, $W_K[D,H_{kv}d_k]$, and $W_V[D,H_{kv}d_v]$. Batched multiplication yields $[B,T,Hd]$; reshape to $[B,T,H,d]$ then permute to head-major $[B,H,T,d]$. For cross-attention, queries come from decoder $X_q[B,T,D_q]$ while K/V come from encoder memory $X_m[B,S,D_m]$ with separate input widths.

$$
S_{attn}=QK^T/\sqrt{d_k}\in\mathbb R^{B\times H_q\times T\times S},\qquad
O=\operatorname{softmax}_S(S_{attn})V.
$$

MHA has $H_{kv}=H_q$. GQA maps each query head $h$ to KV head $\lfloor h/(H_q/H_{kv})\rfloor$ without physically repeating cache. Output $[B,H_q,T,d_v]$ transposes to $[B,T,H_qd_v]$ then multiplies $W_O[H_qd_v,D]$.

```text
X [B,T,D] -> projection [B,T,H*d] -> reshape [B,T,H,d] -> transpose [B,H,T,d]
```
* **Production Reality & Tradeoffs:** A wrong transpose can pass shape checks while mixing token/head axes. Noncontiguous views require reshape/contiguous-aware kernels. Tensor parallelism shards projection/output dimensions and adds collectives. Assert dimensions at every boundary.
* **Red Flag:** Writing $QK$ instead of $QK^T$, omitting sequence $S$, or assuming query and KV head counts must match.

