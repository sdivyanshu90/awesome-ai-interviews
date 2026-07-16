### Q: Derive RoPE rotations and explain long-context extrapolation and scaling failure modes.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** RoPE rotates each query/key feature pair by a position-dependent angle. Dot products then depend on relative offset because $R(m)^TR(n)=R(n-m)$, while values remain unrotated. Extrapolation fails when unseen phase/frequency and attention distributions exceed training.
* **The Deep Dive:** For each 2D pair with frequency $\omega_i=\theta^{-2i/d}$, define

$$
R_i(p)=\begin{bmatrix}\cos(p\omega_i)&-\sin(p\omega_i)\\\sin(p\omega_i)&\cos(p\omega_i)\end{bmatrix},
\quad q'_i=R_i(p)q_i,\quad k'_i=R_i(s)k_i.
$$

Then $(q'_i)^Tk'_i=q_i^TR_i(s-p)k_i$: absolute positions enter rotations but attention scores expose relative displacement. RoPE is applied after Q/K projection, before score matmul, per head; cached keys are stored already rotated at their positions. Different frequency pairs cover different spatial scales.

At lengths beyond training, high-frequency angles wrap and low-frequency phases enter unseen regimes; the model also lacks learned behavior for long-distance distractors even if rotations are mathematically defined. Position interpolation maps new positions $p$ to $p/s$ inside the trained range but compresses relative distances. Base/frequency scaling, NTK-aware variants, YaRN-like piecewise scaling, and continued long-context training trade local resolution against range. Off-by-one positions break KV reuse and chunked decoding.
* **Production Reality & Tradeoffs:** Context extension increases KV memory and prefill cost and may preserve perplexity while losing retrieval. Evaluate local tasks, multiple evidence positions, distractors, multi-hop reasoning, and latency. Changing RoPE parameters usually requires cache invalidation and often fine-tuning.
* **Red Flag:** Saying RoPE simply adds position embeddings, or assuming a larger configured context automatically yields usable long-context reasoning.

