### Q: Compare FP8/INT8/INT4 quantization, GPTQ, AWQ, SmoothQuant, and quantization-aware training.
* **Difficulty:** Senior
* **Category:** Compression
* **The 10-Second Pitch:** Precision formats trade exponent/mantissa or integer range; PTQ methods choose scales and compensate error from calibration, while QAT trains through fake quantization. GPTQ is weight-reconstruction/Hessian-aware, AWQ protects salient channels, and SmoothQuant shifts activation outliers into weights.
* **The Deep Dive:** Quantization maps values to grid $q=\operatorname{clip}(\operatorname{round}(x/s)+z)$ and reconstructs $s(q-z)$, with per-tensor/channel/group scales. FP8 preserves floating exponent with limited mantissa and dynamic scaling; INT8 commonly supports weight+activation; INT4 often weight-only because activations/outliers are harder. Accumulators remain wider.

GPTQ quantizes weights sequentially/blocked while using approximate second-order activation statistics to compensate reconstruction error. AWQ observes activation salience and rescales/protects important weight channels before low-bit weight quantization. SmoothQuant applies an equivalent channel scaling $XW=(XS^{-1})(SW)$ to move activation range into weights, making INT8 activations easier. QAT inserts fake quant/dequant in forward and STE-like gradients so weights adapt; it costs training but handles aggressive schemes better.

Evaluate perplexity and task/safety/tool/code/long-context slices, layer errors, outliers, saturation, and actual kernels. KV-cache quantization is separate.
* **Production Reality & Tradeoffs:** Nominal 4-bit storage includes scales/zeros and may not speed unsupported shapes/hardware. Dequantization, small batches, and bandwidth/compute balance set latency. Calibration data must match rare long/tool workloads. Quantization methods and formats evolve; pin runtime.
* **Red Flag:** Ranking methods by bit width alone or assuming smaller checkpoints guarantee faster serving with unchanged quality.

