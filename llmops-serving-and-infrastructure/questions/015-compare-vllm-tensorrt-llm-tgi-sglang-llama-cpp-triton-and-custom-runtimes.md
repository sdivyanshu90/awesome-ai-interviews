### Q: Compare vLLM, TensorRT-LLM, TGI, SGLang, llama.cpp, Triton, and custom runtimes.
* **Difficulty:** Senior
* **Category:** Serving
* **The 10-Second Pitch:** Choose by hardware, supported models/features, performance, deployment complexity, portability, and control. vLLM/SGLang emphasize flexible high-throughput serving; TensorRT-LLM deep NVIDIA optimization; TGI integrated serving; llama.cpp CPU/edge portability; Triton is a general server/kernel language name requiring clarification.
* **The Deep Dive:** Compare continuous batching, paged/prefix KV, quantization, speculative/structured decoding, LoRA, multimodal, distributed modes, observability, API compatibility, release cadence, and kernel support. NVIDIA Triton Inference Server differs from OpenAI Triton kernel language. Custom runtimes justify themselves only for workload-specific constraints.
* **Production Reality & Tradeoffs:** Benchmarks are version/config/hardware-sensitive. Operational maturity, upgrade burden, and correctness can outweigh small throughput wins.
Choose a runtime through an empirical compatibility matrix: model/architecture, GPU or CPU, precision, tensor/expert parallelism, structured decoding, adapters, speculative decoding, multimodal inputs, operational APIs, and observability. vLLM/SGLang emphasize high-throughput scheduling and reuse; TensorRT-LLM emphasizes NVIDIA engine optimization; TGI packages production serving; llama.cpp targets portable local inference; Triton is a kernel language/server name and custom runtimes maximize control. Benchmark exact releases; feature support changes rapidly.

* **Red Flag:** Choosing from one vendor benchmark without matching workload.
