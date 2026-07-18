### Q: Design 1D/2D/3D/4D parallelism for dense and MoE models across nodes.
* **Difficulty:** Principal
* **Category:** Distributed Training
* **The 10-Second Pitch:** Choose axes to fit memory and keep frequent high-volume communication on fastest links: tensor/expert within NVLink domains, pipeline across suitable stage groups, data/ZeRO across replicas, and context parallel for long sequences.
* **The Deep Dive:** Begin with a memory and communication ledger per layer: parameter/gradient/optimizer bytes, saved activations, temporary buffers, and collective volume/frequency. One-dimensional training chooses one main axis, usually data or tensor parallel. Two-dimensional composes, for example, TP$\times$DP; three-dimensional commonly uses TP$\times$PP$\times$DP; a fourth axis may be context/sequence or expert parallelism. Degrees must multiply to world size and process groups must be orthogonal. The global batch is $B_{global}=B_{micro/rank}\,N_{accum}\,N_{DP}$; tensor and pipeline parallel degrees do not multiply the statistical batch.

```text
fast NVLink/NVSwitch island (node)
  [TP0 TP1 TP2 TP3] = one pipeline stage, frequent per-layer collectives
          | pipeline activations
  [TP0 TP1 TP2 TP3] = next stage

repeat stage group across nodes/racks -> DP replicas, one reduction per accumulated step
MoE: EP group routes tokens with all-to-all; place it on the fastest feasible fabric
```

  TP splits matrix dimensions and communicates activations every layer, so keep the group on the fastest links. PP partitions contiguous layers and sends activations/gradients between stages; balance measured stage time and use enough microbatches to amortize bubbles. DP/FSDP replicas can span slower links because gradient/state communication occurs less frequently and can overlap. Context parallel shards sequence state but introduces attention communication. Expert parallel dispatches tokens via all-to-all; align expert groups with topology, use grouped GEMMs, capacity controls, and router balancing. Shared/dense experts may themselves use TP.
* **Production Reality & Tradeoffs:** A configuration that fits can still be slower due to PP bubbles, TP latency, all-to-all oversubscription, expert skew, or too-small local GEMMs. Model topology including NIC/GPU affinity and concurrent collectives, not nominal bandwidth alone. Checkpoints must record logical tensors so a new mesh can reshard. Benchmark a representative layer and several full steps at candidate meshes; measure per-rank memory, stage time, collective tails, overlap, and tokens/s before allocating the cluster.
* **Red Flag:** Multiplying parallel degrees until memory fits without communication modeling.
