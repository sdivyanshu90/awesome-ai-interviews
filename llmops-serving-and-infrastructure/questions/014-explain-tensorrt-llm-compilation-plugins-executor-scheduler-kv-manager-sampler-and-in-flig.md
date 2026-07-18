### Q: Explain TensorRT-LLM compilation, plugins, executor, scheduler, KV manager, sampler, and in-flight batching.
* **Difficulty:** Senior
* **Category:** Serving
* **The 10-Second Pitch:** TensorRT-LLM compiles model graphs into optimized engines/plugins and uses an executor loop whose scheduler selects requests, KV manager allocates blocks, model engine runs, and sampler produces tokens under in-flight batching.
* **The Deep Dive:** Build converts weights/config to supported graph, selects precision and fused plugins for attention/MoE/GEMM, and may capture CUDA graphs. Runtime queues requests; scheduler mixes context/generation tokens; KV manager manages paged/contiguous pools and reuse/offload; model workers execute across ranks; sampler applies decoding. C++ runtime reduces host overhead. Engine profiles and plugin support constrain shapes/features.
* **Production Reality & Tradeoffs:** Compilation time/artifacts tie to GPU, precision, model, and limits. Unsupported custom ops or dynamic patterns can force fallback. Tune memory and scheduler using real workloads.
TensorRT-LLM transforms a supported model into optimized engines using precision choices and plugins, then an executor manages requests, in-flight batching, KV, sampling, and distributed ranks. Build-time specialization can improve kernels but creates engine/profile compatibility and cold-build costs. Dynamic shapes, LoRA, speculative modes, quantization, and parallel topology need explicit support. Validate output parity and performance for every engine profile; falling outside a captured profile can rebuild, reject, or use a slower path.

* **Red Flag:** Treating TensorRT-LLM as merely exporting PyTorch to ONNX.
