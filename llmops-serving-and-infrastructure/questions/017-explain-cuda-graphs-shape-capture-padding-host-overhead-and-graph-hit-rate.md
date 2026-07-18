### Q: Explain CUDA Graphs, shape capture, padding, host overhead, and graph hit rate.
* **Difficulty:** Senior
* **Category:** GPU
* **The 10-Second Pitch:** CUDA Graphs capture a fixed launch DAG and replay it with low CPU/driver overhead. Dynamic serving pads/maps work to captured shapes; hit rate determines benefit.
* **The Deep Dive:** Warmup allocates stable addresses and captures representative batch/token shapes. Runtime selects nearest compatible graph, updates input buffers, and replays. Padding wastes GPU work but can beat eager launches. Decode's many small repeated kernels benefits most; dynamic control, memory allocation, custom ops, or uncommon shapes cause fallback. Maintain multiple graph sizes under memory limits.
* **Production Reality & Tradeoffs:** Graphs reserve memory and complicate cancellation/scheduling. Capture overhead and low hit rate can hurt. Track graph selection, padding ratio, fallback, and E2E gains.
CUDA Graphs record a fixed launch graph and replay it with low CPU overhead. Pointers, control flow, and often shapes must remain stable; serving therefore buckets batch/token shapes, uses static memory pools, and pads to captured profiles. Graph hit rate determines real gain. Excessive buckets consume memory and warmup time, while padding wastes GPU work. Keep an eager fallback and measure end-to-end p99—microbenchmarked replay speed can be erased by misses and scheduler complexity.

* **Red Flag:** Saying graphs fuse kernels or support arbitrary dynamic shapes.
