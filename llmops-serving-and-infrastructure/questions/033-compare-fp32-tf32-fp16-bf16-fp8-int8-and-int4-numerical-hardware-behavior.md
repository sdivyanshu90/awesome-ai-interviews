### Q: Compare FP32, TF32, FP16, BF16, FP8, INT8, and INT4 numerical/hardware behavior.
* **Difficulty:** Senior
* **Category:** Numerics
* **The 10-Second Pitch:** Formats trade exponent, mantissa, integer scaling, memory, and tensor-core throughput. BF16 preserves FP32 range; FP16 has more fraction but narrow range; TF32 accelerates FP32 matmuls; FP8/INT formats require scaling/calibration.
* **The Deep Dive:** FP32 is baseline. TF32 uses reduced mantissa internally on NVIDIA tensor cores while inputs/outputs appear FP32. FP16 often needs loss scaling; BF16 usually avoids overflow but has coarse precision. FP8 variants balance exponent/mantissa with dynamic scaling. INT8/4 use affine/symmetric quantization and accumulate higher precision. Reductions/norms/optimizer often remain FP32.
* **Production Reality & Tradeoffs:** The fastest supported format depends on GPU/kernel/shape. Numerical stability and quality are operation-specific. Verify overflow, underflow, saturation, and convergence.
FP32 maximizes common precision; TF32 keeps FP32 range with reduced mantissa for tensor-core matmul; FP16 has limited exponent range and often needs loss scaling; BF16 keeps FP32-like exponent range; FP8 formats trade exponent/mantissa and require scaling; INT8/INT4 represent quantized integers with group scales/zeros. Storage dtype, accumulation dtype, and communication dtype can differ. Evaluate overflow, underflow, outliers, reductions, optimizer states, and hardware kernel support separately.

* **Red Flag:** Ranking formats by bit count alone.
