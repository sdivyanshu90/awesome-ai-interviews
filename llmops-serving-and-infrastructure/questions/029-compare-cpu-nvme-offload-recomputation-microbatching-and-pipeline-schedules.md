### Q: Compare CPU/NVMe offload, recomputation, microbatching, and pipeline schedules.
* **Difficulty:** Principal
* **Category:** Distributed Training
* **The 10-Second Pitch:** All reduce accelerator memory differently: offload moves state to slower tiers, recomputation trades extra FLOPs, microbatching reduces activation peak, and pipeline schedules trade bubbles/state overlap.
* **The Deep Dive:** CPU offload moves optimizer/parameters/activations over PCIe; NVMe extends capacity with much higher latency and prefetch requirements. Recomputation reruns layers. Smaller microbatches lower memory but reduce GEMM efficiency and change normalization/noise. GPipe fills then drains; 1F1B alternates forward/backward with fewer stored activations; interleaving virtual stages reduces bubbles but adds communication.
* **Production Reality & Tradeoffs:** The cheapest theoretical tier may bottleneck wall time. Profile transfer overlap, pinned memory, SSD endurance, bubbles, and host RAM. Preserve global batch semantics.
CPU/NVMe offload increases capacity but transfers over slower links; recomputation exchanges FLOPs for activation memory; microbatching lowers peak activations and enables pipeline schedules but changes utilization/optimizer dynamics. GPipe-style schedules have fill/drain bubbles; 1F1B reduces live activations; interleaving reduces bubbles with complexity. Build a byte-and-time budget for transfers and overlap. Offloading a tensor needed every layer/step often converts an OOM into an unusably slow job.

* **Red Flag:** Using offload to avoid capacity planning while ignoring PCIe bandwidth.
