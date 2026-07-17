### Q: Derive $W'=W+(\alpha/r)BA$, initialization, scaling, rank, dropout, and merge semantics.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** LoRA freezes $W$ and learns a rank-at-most-$r$ update $BA$, scaled by $\alpha/r$. One factor is commonly zero-initialized so the initial model is unchanged; merging adds the trained update into $W$ for inference.
* **The Deep Dive:** For $W\in\mathbb R^{d_{out}\times d_{in}}$, choose $A\in\mathbb R^{r\times d_{in}}$ and $B\in\mathbb R^{d_{out}\times r}$. The adapted layer is

$$
y=Wx+\frac{\alpha}{r}B(Ax),\qquad
W'=W+\Delta W,\quad \Delta W=\frac{\alpha}{r}BA.
$$

The adapter has $r(d_{in}+d_{out})$ trainable parameters and $\operatorname{rank}(\Delta W)\le r$. Dropout may apply on the adapter input during training. Initialize one factor randomly and the other to zero so $\Delta W=0$ initially but gradients enter the zero factor; initializing both to zero blocks both gradients. Merging is algebraically exact in real arithmetic but finite-precision merge/unmerge accumulates rounding.

  ```text
                         frozen path
  x ─────────────────────── W ───────────────────┐
  │                                              ├─> y
  └─ dropout ─> A [r×d_in] ─> B [d_out×r] ─> α/r┘
                    trainable low-rank update ΔW = (α/r)BA
  ```
* **Production Reality & Tradeoffs:** Merged weights remove adapter matmuls but prevent cheap switching. Quantized bases may require dequantize-merge-requantize and can lose accuracy. Scaling conventions differ across libraries/checkpoints.
* **Red Flag:** Saying rank $r$ means only $r$ original neurons are trained.
