### Q: Explain QLoRA, NF4, double quantization, paged optimizers, and gradient flow through frozen weights.
* **Difficulty:** Senior
* **Category:** Training
* **The 10-Second Pitch:** QLoRA stores frozen base weights in 4-bit NF4, dequantizes them for matmuls, and trains higher-precision LoRA parameters. Double quantization compresses scale constants; paged optimizers manage memory spikes. Base weights receive no optimizer update.
* **The Deep Dive:** NF4 places quantization levels to suit approximately normal weights, typically with blockwise scales. Quantized weights are dequantized to compute dtype during forward; autograd propagates gradients through activations into LoRA A/B, while base parameters are frozen. Double quantization quantizes the per-block quantization constants. Paged optimizer state can spill/manage memory via unified-memory mechanisms when long sequences cause peaks. Compute is not truly 4-bit throughout.
* **Production Reality & Tradeoffs:** QLoRA saves weight memory, not all activations/KV/adapter optimizer memory. Kernel/device support and calibration matter. Do not merge adapters directly into low-bit codes without a deliberate requantization path.
Numerically, dequantization is part of the forward graph but not a trainable parameterization: $W_q$ is decoded blockwise to the compute dtype, used in matmul, and discarded or cached; only adapter tensors receive gradients and optimizer states. Validate peak memory with long sequences because activations and temporary dequantization buffers may dominate the advertised 4-bit storage savings.

* **Red Flag:** Saying gradients update the 4-bit base weights.
