### Q: Explain sinusoidal position encoding and its relative-position algebra.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Sinusoidal encoding assigns position to sine/cosine pairs at log-spaced frequencies. A fixed offset is a 2D rotation of each pair, so relative shifts are linearly recoverable even though the original Transformer adds absolute vectors.
* **The Deep Dive:** For pair $i$ with frequency $\omega_i=10000^{-2i/D}$,

$$
p_i(t)=\begin{bmatrix}\sin(\omega_it)\\\cos(\omega_it)\end{bmatrix}.
$$

Angle-addition gives $p_i(t+k)=R_i(k)p_i(t)$ for a rotation matrix depending only on $k$. Dot products also satisfy $p_i(t)^Tp_i(s)=\cos(\omega_i(t-s))$. Multiple frequencies cover fine-to-coarse scales and help distinguish positions over the designed range. In the original Transformer, $p(t)$ is added to token embeddings, so content and position occupy the same residual dimensions and learned projections must extract useful relations.

Unlike RoPE, sinusoidal encoding does not directly rotate Q/K after projection; its relative algebra is available to the network but not hard-coded into attention scores. Mathematically defined positions beyond training do not guarantee behavioral extrapolation because downstream layers saw only a limited phase/distance distribution. Very large positions face finite-precision/periodicity collisions.
* **Production Reality & Tradeoffs:** No learned position parameters and variable length are advantages; addition can entangle content and position and may underperform relative schemes. Padding/offset conventions and interpolation must be consistent across training/cache.
* **Red Flag:** Saying each position has a one-hot encoding or that sinusoidal formulas guarantee unlimited context extrapolation.

