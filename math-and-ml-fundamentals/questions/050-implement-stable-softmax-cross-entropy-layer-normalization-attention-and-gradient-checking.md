### Q: Implement stable softmax, cross-entropy, layer normalization, attention, and gradient checking from scratch.
* **Difficulty:** Mid
* **Category:** Coding
* **The 10-Second Pitch:** Implement stable primitives with explicit axes, shapes, masks, and accumulation dtype; verify forward values against a trusted reference and backward derivatives with float64 directional finite differences across adversarial edge cases.
* **The Deep Dive:** Softmax subtracts the row maximum; CE from logits uses $-z_y+\operatorname{LSE}(z)$ without materializing probabilities for the loss. LayerNorm computes per-token mean/variance in a wider accumulator, then affine scale/shift. Attention projects/reshapes $[B,T,D]\to[B,H,T,d_h]$, forms $QK^T/\sqrt{d_h}$, applies causal/padding mask before key-axis softmax, multiplies $V$, merges heads, and output-projects.

```python
def logsumexp(z, axis=-1, keepdims=False):
    m = z.max(axis=axis, keepdims=True)
    out = m + log(exp(z - m).sum(axis=axis, keepdims=True))
    return out if keepdims else squeeze(out, axis)

def cross_entropy(logits, target):
    return mean(-take_along_axis(logits, target[:, None], -1)[:, 0]
                + logsumexp(logits, -1))
```

Define fully masked-row behavior explicitly rather than softmaxing all $-\infty$. Gradient checks use float64, centered differences/directions, an epsilon sweep, and relative plus absolute error. Tests include extreme/equal logits, odd/noncontiguous shapes, singleton axes, causal+padding masks, repeated operands, and normalization near zero variance.
* **Production Reality & Tradeoffs:** A pedagogical implementation is not a performant kernel. Production needs fused operations, tensor-core layouts, deterministic options, mixed-precision error bounds, and no intermediate score matrix for FlashAttention-style algorithms. Optimize only after reference parity.
* **Red Flag:** Computing `exp(logits)` directly, normalizing attention over queries, or declaring gradients correct from one float32 finite-difference point.

