### Q: Derive Vision Transformer patch embeddings, class tokens, positional embeddings, and attention shapes.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** Split an image into patches, flatten and project each patch to model width, optionally prepend a learned class token, add position embeddings, then apply Transformer attention over the resulting sequence.
* **The Deep Dive:** For image $[B,C,H,W]$, patch size $P$, $N=(H/P)(W/P)$. A strided $Conv2d(C,D,kernel=P,stride=P)$ is equivalent to linear patch projection and yields $[B,D,H/P,W/P]$, reshaped to $[B,N,D]$. Prepending CLS gives $T=N+1$. Q/K/V become $[B,h,T,d_h]$; attention scores are $[B,h,T,T]$. CLS pools through attention; alternatives mean-pool patch tokens. Learned 2D positions are usually interpolated for new resolutions, which changes geometry.
* **Production Reality & Tradeoffs:** Token count grows with image area and dominates memory. Position interpolation, padding, variable aspect ratios, and nondivisible dimensions need explicit policies. CLS is not inherently superior to pooling.
For image $H\times W$, patch $P$, and $C$ channels, flattening yields $N=HW/P^2$ patches and projection $X_p\in\mathbb R^{N\times(P^2C)}E$, $E\in\mathbb R^{P^2C\times D}$. After prepending CLS, attention sees $(B,N+1,D)$; per head, $Q,K,V$ are $(B,h,N+1,d_h)$. Positional embeddings encode ordering that patchification removed. Changing resolution requires interpolation or relative positions and changes quadratic attention cost.

* **Red Flag:** Saying ViT attends directly to raw pixels without a learned patch projection.
