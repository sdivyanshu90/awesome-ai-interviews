### Q: Why did recurrence and convolution motivate the original Transformer architecture?
* **Difficulty:** Mid
* **Category:** Conceptual
* **The 10-Second Pitch:** RNNs impose sequential paths that limit training parallelism and long-range gradient distance; convolutions parallelize but require depth/dilation for distant interaction. Self-attention connects any pair in one layer and parallelizes positions, trading this for quadratic dense attention.
* **The Deep Dive:** For sequence length $T$, an RNN has $O(T)$ sequential operations and path length $O(T)$ between distant tokens, although per-step cost is linear. A fixed-kernel convolution parallelizes positions but distant information needs $O(T/k)$ layers or $O(\log T)$ with dilation; locality is a strong inductive bias. Dense self-attention computes pairwise interactions in $O(1)$ sequential depth with path length one, and Q/K content determines which positions interact.

The original Transformer therefore used multi-head self-attention for token mixing, position-wise feed-forward networks for nonlinear channel transformation, residual connections and LayerNorm for optimization, explicit positional encodings because attention is permutation-equivariant, and encoder-decoder cross-attention for conditional translation. The decoder’s causal mask preserves autoregressive factorization.

| Layer | Sequential depth | Max path | Per-layer interaction cost |
|---|---:|---:|---:|
| recurrent | $T$ | $T$ | $O(Td^2)$ |
| convolution | 1 | depth-dependent | $O(kTd^2)$ |
| dense attention | 1 | 1 | $O(T^2d)$ |
* **Production Reality & Tradeoffs:** Attention’s $T^2$ compute/memory and growing KV cache dominate long sequences; recurrence/SSMs can stream fixed state and CNNs bring useful locality. The Transformer won through quality, parallel hardware use, and scaling—not universal asymptotic superiority.
* **Red Flag:** Saying attention has linear cost or that Transformers have no sequential dependence during autoregressive inference.

