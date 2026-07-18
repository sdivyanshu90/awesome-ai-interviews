### Q: Compare data, tensor, pipeline, sequence/context, expert, and optimizer-state parallelism.
* **Difficulty:** Senior
* **Category:** Distributed Training
* **The 10-Second Pitch:** Parallelism shards different dimensions: examples, layer tensors, layer stages, sequence positions, experts, or optimizer state. Large training composes them to fit memory and minimize communication/bubbles.
* **The Deep Dive:** Data parallel replicates model and reduces gradients. Tensor parallel splits matmuls/heads and communicates activations each layer. Pipeline partitions layers and schedules microbatches. Sequence/context parallel shards token-axis activations/attention. Expert parallel assigns MoE experts and all-to-all routes tokens. ZeRO/FSDP shards parameters, gradients, and optimizer state across data ranks. Product of dimensions forms world size; group topology matters.
* **Production Reality & Tradeoffs:** Each dimension changes batch semantics, checkpoint layout, failure domain, and collectives. More dimensions add complexity and may hurt utilization. Select from model shape/topology/profile.
Data parallelism replicates the model; tensor parallelism shards layer matmuls with per-layer collectives; pipeline parallelism partitions layers and introduces bubbles; sequence/context parallelism shards token-dependent activations/attention; expert parallelism routes tokens across MoE ranks; optimizer-state parallelism shards training states. Select from memory fit and communication topology. Tensor/expert parallel traffic should stay on fast local fabrics when possible, while data-parallel synchronization crosses nodes less frequently.

* **Red Flag:** Listing names without their communication pattern.
