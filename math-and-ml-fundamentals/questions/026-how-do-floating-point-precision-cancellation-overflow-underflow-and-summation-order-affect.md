### Q: How do floating-point precision, cancellation, overflow, underflow, and summation order affect ML kernels?
* **Difficulty:** Principal
* **Category:** Numerical Methods
* **The 10-Second Pitch:** Floating point has finite exponent and significand, so rounding accumulates, addition is nonassociative, exponentials overflow/underflow, and subtracting close values destroys relative precision. Stable kernels reformulate math and accumulate critical reductions wider.
* **The Deep Dive:** A normalized floating value is approximately $(-1)^s(1.f)_2 2^e$. FP16 has narrow exponent range; BF16 retains FP32-like range with fewer precision bits. Machine epsilon bounds relative rounding near one, but subnormals/flush-to-zero complicate tiny values. Since $(a+b)+c\ne a+(b+c)$, GPU reduction trees produce different last bits across shapes/ranks.

Stable softmax uses

$$
\operatorname{softmax}(z)_i=\frac{e^{z_i-m}}{\sum_j e^{z_j-m}},\qquad m=\max_jz_j,
$$

and log-sum-exp is $m+\log\sum_j e^{z_j-m}$. Variance via $E[x^2]-E[x]^2$ catastrophically cancels when terms are close; Welford/two-pass algorithms are safer. Pairwise or Kahan summation reduces accumulation error. Mixed precision stores operands low but accumulates GEMM/reductions FP32; loss scaling moves small FP16 gradients into range, then unscales before clipping and checks overflow. FP8-native pretraining—FP8 GEMM operands with higher-precision master weights and per-tensor scaling factors—is now mainstream on H100/Blackwell-class hardware.

Stress tests include extreme logits, almost-constant variance, long reductions, and cross-device order changes.
* **Production Reality & Tradeoffs:** More stable algorithms may require extra passes, memory, or synchronization. Deterministic reductions reduce performance. Epsilon changes both forward and gradients; choose it per dtype/scale. Quantization adds scale/zero-point saturation and accumulator overflow considerations.
* **Red Flag:** Saying BF16 is simply more precise than FP16, or fixing NaNs by adding arbitrary epsilon without locating the unstable operation.

