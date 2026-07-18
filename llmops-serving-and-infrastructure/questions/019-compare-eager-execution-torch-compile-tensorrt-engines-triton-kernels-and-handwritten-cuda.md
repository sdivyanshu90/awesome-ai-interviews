### Q: Compare eager execution, torch.compile, TensorRT engines, Triton kernels, and handwritten CUDA.
* **Difficulty:** Principal
* **Category:** GPU
* **The 10-Second Pitch:** Eager maximizes flexibility; graph compilers optimize framework programs; TensorRT builds specialized inference engines; Triton expresses custom GPU kernels productively; CUDA offers maximum low-level control. Specialization trades development/compile cost for performance.
* **The Deep Dive:** `torch.compile` captures/guards graphs and lowers through backends, with breaks/recompilation for dynamics. TensorRT selects/fuses tactics under profiles and plugins. Triton exposes blocked tensor programming and autotuning but still requires memory/numerical expertise. CUDA controls warps/shared/registers/instructions and supports unusual features at high engineering cost. Compose them rather than view as exclusive.
* **Production Reality & Tradeoffs:** Maintain correctness tests, fallbacks, hardware matrices, and upgrade ownership. Custom kernels are justified by dominant stable hotspots. Compiler wins vary by shape.
Eager execution maximizes flexibility and debuggability; torch.compile captures graphs and generates kernels with graph-break risk; TensorRT specializes engines/profiles; Triton expresses tiled GPU kernels productively; handwritten CUDA offers maximum control at highest maintenance cost. Escalate only after profiling identifies a stable hot path. Require numerical parity, dynamic-shape coverage, fallback, build reproducibility, and hardware/version CI. A faster isolated kernel can slow the request through conversions or lost batching.

* **Red Flag:** Handwriting CUDA before profiling the actual bottleneck.
