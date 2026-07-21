### Q: Explain the modern training-stability toolbox: QK-norm, attention-logit soft-capping, z-loss, and residual-scale controls.
* **Difficulty:** Senior
* **Category:** Training
* **The 10-Second Pitch:** Each tool bounds a quantity transformers let drift: QK-norm pins query/key magnitudes so attention logits cannot saturate into one-hot softmax, soft-capping squashes logits through a scaled tanh, z-loss tethers the softmax normalizer cross-entropy leaves unconstrained, and residual-scale controls stop stream variance growing with depth — loss-spike telemetry tells you which one you need.
* **The Deep Dive:** Attention-logit explosion is the canonical instability: nothing bounds $\|q\|\,\|k\|$, and once logits are large the softmax saturates, its gradient (proportional to $p(1-p)$) vanishes, and the head stops learning while risking overflow. Worked numbers with $d_h = 128$: if per-coordinate RMS of $q$ and $k$ drifts to 8, the peak logit reaches $\|q\|\|k\|/\sqrt{d_h} = (8\sqrt{128})(8\sqrt{128})/\sqrt{128} = 64\sqrt{128} \approx 724$, and any logit gap above roughly 20 is already numerically one-hot. QK-norm applies RMSNorm (with learnable gain) to queries and keys per head before the dot product,

  $$
  \ell_{st} = \frac{\mathrm{RMSNorm}(q_s) \cdot \mathrm{RMSNorm}(k_t)}{\sqrt{d_h}},
  $$

  which pins the RMS to 1 and bounds $|\ell_{st}| \le \sqrt{d_h} \approx 11.3$ by Cauchy–Schwarz — logit scale becomes a trained gain, not a runaway product. ViT-22B could not train without it, and OLMo 2 and Gemma 3 adopt it. Soft-capping is the blunter alternative: $\ell \leftarrow c \tanh(\ell/c)$ bounds $|\ell| < c$ smoothly (Gemma 2: $c = 50$ attention, $c = 30$ final logits; the 724 above maps to essentially 50), with derivative $\mathrm{sech}^2(\ell/c)$ that decays but never hits zero, unlike hard clipping. Its cost is kernel friction — capped attention missed stock FlashAttention fast paths — one reason Gemma 3 swapped it for QK-norm.

  z-loss targets a different flat direction: cross-entropy is invariant under a uniform logit shift $z \mapsto z + c\mathbf{1}$, so the scale of $Z = \sum_j e^{z_j}$ is unconstrained and drifts upward until $e^{z}$ overflows low-precision compute. The fix (PaLM; router z-loss in ST-MoE) adds

  $$
  \mathcal{L}_z = \lambda \log^2 Z, \qquad \frac{\partial \mathcal{L}_z}{\partial z_j} = 2\lambda (\log Z)\, p_j, \qquad \lambda \approx 10^{-4},
  $$

  a uniform pull-down proportional to $\log Z$'s distance from 0 — at $\log Z = 5$ the added gradient is only $10^{-3} p_j$, yet it removes the drift. Residual-scale controls address depth: with pre-norm, $x_{l+1} = x_l + f_l(x_l)$, and for roughly uncorrelated block outputs of variance $\sigma_f^2$ the stream variance grows like $\mathrm{Var}(x_0) + L\sigma_f^2$, so RMS scales as $\sqrt{L}$ and late layers run hottest. Controls: scale residual-branch output-projection init by $1/\sqrt{2L}$ (GPT-2 style, $2L$ additions per stream), learnable per-channel LayerScale with tiny init, or post-block norms alongside pre-norm (Gemma 2's sandwich).

  Telemetry decides which tool you need — each failure has a distinct signature before the spike:

  ```text
  signature                                          failure               tool
  max attention logit grows; head entropy -> 0       logit explosion       QK-norm / attn soft-cap
  log Z of output softmax drifts up over steps       output-logit drift    z-loss
  activation RMS grows with layer index and time;    residual blowup       1/sqrt(2L) init, LayerScale,
  spikes originate in late layers                                          post-block norm
  ```

  The falsifiable test comes from small-scale proxies (Wortsman et al., 2023): attention-logit growth and output-logit divergence both reproduce in small models at high learning rate, and QK-norm and z-loss each measurably extend the stable learning-rate range. Run the sweep with and without the intervention; if the divergence threshold does not move, the spike has another cause. The limiting case: a spike coincident with a data-shard boundary that survives all four tools falsifies architectural instability as the diagnosis — the remedy is data-side (skip the batch, rewind), not another bound.
* **Production Reality & Tradeoffs:** These tools are cheap in FLOPs but not free: QK-norm adds per-head norms that need fusing to avoid decode overhead, soft-capping constrains kernel choice, z-loss is one more coefficient to thread through resume logic, and residual-scale choices interact with muP-style scaling rules. Frontier practice layers cheap prevention (QK-norm and z-loss on by default) with monitoring — logging max attention logit, $\log Z$, and per-layer activation RMS costs almost nothing and converts a mysterious spike into a diagnosis — plus fallbacks, since skip-batch and rewind handle the spikes no bound prevents.
* **Red Flag:** Treating the tools as interchangeable spike cures — z-loss cannot fix attention-logit explosion and QK-norm cannot fix output-logit drift; they bound different quantities — claiming cross-entropy alone constrains logit scale when it is shift-invariant, or prescribing soft-capping without noting the kernel-compatibility cost and that QK-norm has largely superseded it on the attention path.
