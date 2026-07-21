### Q: Explain low-precision pretraining in BF16 and FP8: master weights, scaling strategies, and which tensors stay in high precision.
* **Difficulty:** Senior
* **Category:** Systems
* **The 10-Second Pitch:** Run GEMMs in the cheapest format that preserves signal — BF16 broadly (its 8-bit exponent matches FP32's range), FP8 with E4M3 forward and E5M2 gradients — while master weights, accumulators, norms, softmax, and the optimizer chain stay high precision; scales steer tensors into FP8's narrow window, and DeepSeek-V3 is the frontier-scale existence proof.
* **The Deep Dive:** BF16 keeps FP32's 8-bit exponent and cuts the mantissa to 7 bits: range matches FP32 but unit roundoff is $u = 2^{-8} \approx 0.4\%$. Range parity is why BF16 needs no loss scaling while FP16 (5-bit exponent, max 65504) underflows small gradients without one. The mantissa cut is why master weights stay FP32: a weight update is lost to rounding when

  $$
  |\Delta w| \;<\; \tfrac{u}{2}\,|w|,
  $$

  and Adam updates at typical learning rates have $|\Delta w|/|w| \sim 10^{-4}$ to $10^{-3}$, below BF16's $u/2 \approx 2 \times 10^{-3}$ — storing weights only in BF16 silently freezes slow-moving coordinates, so the optimizer updates an FP32 copy, cast down for compute.

  FP8 splits into two formats: E4M3 (max 448, min normal $2^{-6}$, subnormals to $2^{-9}$, half-ulp error $2^{-4} = 6.25\%$) for forward weights and activations, and E5M2 (max 57344) for backward gradients, which need range more than precision. Because the representable window is tiny, every FP8 tensor carries a scale $s$ with $x_q = \mathrm{cast}(s\,x)$, chosen so the scaled amax sits near format max. Delayed per-tensor scaling picks $s$ from an amax history — one pass, but a fresh spike clips against the stale scale; just-in-time scaling computes the true amax at the cost of an extra reduction; fine-grained block scaling (DeepSeek-V3: $1 \times 128$ activation tiles, $128 \times 128$ weight blocks, scales computed online) shrinks the blast radius of outliers. Worked example: a tensor with bulk values near 0.02 and amax 6.5 gets $s = 448/6.5 \approx 69$, so the bulk lands near 1.4 with at most 6.25 percent rounding error; one outlier of 6500 forces $s \approx 0.069$, the bulk lands near 0.0014 — inside the subnormal region with fixed spacing $2^{-9} \approx 0.002$ — and rounds to 0 or 0.002, up to 100 percent error on the bulk. With $1 \times 128$ tiles, only the outlier's own tile pays.

  ```text
  FP32 : master weights, gradient accumulation and all-reduce, GEMM accumulators
         (partial sums promoted off the tensor cores every 128 MACs), norm
         statistics, softmax/attention-logit math, MoE router scores
  BF16 : optimizer moments (DeepSeek-V3), embeddings, output head, most
         non-GEMM elementwise compute
  FP8  : E4M3 weights + activations in forward GEMMs, E5M2 gradients backward
  ```

  Accumulation is the remaining hazard: summing $K$ products with roundoff $u$ grows relative error like $\sqrt{K}\,u$ for random signs and up to $K u$ adversarially, so with inner dimensions $K \sim 10^4$ the accumulator, not the operand format, sets fidelity — hence FP32 accumulation, with DeepSeek-V3 promoting tensor-core partial sums to FP32 every 128 elements. Ablations report relative loss deviation under 0.25 percent versus BF16, and the production run — a 671B-parameter MoE, 37B active, 14.8T tokens — is the existence proof. The falsifiable test is that ablation: FP8 and BF16 proxy runs should overlap within seed noise; removing the FP32 promotion, or collapsing block scales to one per-tensor scale on outlier-heavy activations, reopens a visible loss gap.
* **Production Reality & Tradeoffs:** FP8 roughly doubles GEMM throughput on Hopper-class hardware and shrinks activation memory, but the high-precision holdouts cap end-to-end speedup well below the kernel-level 2x; the engineering tax is scale management (amax tracking, checkpoint compatibility for scales and FP32 masters) and numerics debugging, since a clipped delayed scale corrupts silently rather than NaN-ing. FP8 training is not FP8 inference quantization: training quantizes fresh activations and gradients every step with dynamic scales and errors compound across $10^5$-plus optimizer steps, while post-training quantization is one-shot calibration of a frozen model — though an FP8-trained checkpoint (DeepSeek-V3 ships FP8 weights) makes serving quantization nearly free.
* **Red Flag:** Saying BF16 needs loss scaling (that is FP16's problem), claiming FP8 training means every tensor is FP8 — master weights, accumulators, optimizer states, norms, and softmax are not — or assuming inference-quantization experience transfers to training, where gradient range and per-step error compounding have no analogue.
