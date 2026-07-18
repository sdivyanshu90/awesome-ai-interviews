### Q: Compare PCIe, NVLink, NVSwitch, InfiniBand, RoCE, Ethernet, and topology-aware placement.
* **Difficulty:** Senior
* **Category:** Infrastructure
* **The 10-Second Pitch:** PCIe connects devices/host; NVLink gives high-bandwidth GPU links; NVSwitch creates intra-domain all-to-all; InfiniBand/RoCE provide RDMA across nodes; Ethernet varies. Place chatty parallel groups on fastest/local links.
* **The Deep Dive:** Topology includes NUMA CPU, PCIe root complexes, GPU link graph, NIC affinity, switch oversubscription, and routing. TP/all-to-all benefit from NVLink/NVSwitch; DP can tolerate slower overlapped inter-node links. InfiniBand has dedicated fabric semantics; RoCE runs RDMA over configured lossless/congestion-controlled Ethernet. GPUDirect RDMA avoids host copies.
* **Production Reality & Tradeoffs:** Advertised bandwidth differs from effective collective throughput. Misbinding GPU/NIC/CPU causes cross-socket traffic. Validate topology and collective benchmarks.
PCIe connects host/devices at lower bandwidth than NVLink; NVLink provides fast GPU peer links; NVSwitch supplies high-bisection intra-node connectivity; InfiniBand and RoCE provide high-speed inter-node RDMA, with RoCE depending heavily on Ethernet congestion configuration; ordinary Ethernet may suit control/data-parallel traffic but bottleneck tensor/expert collectives. Map rank groups to measured topology, keep chatty collectives local, and benchmark effective bidirectional bandwidth/latency under concurrent traffic.

* **Red Flag:** Treating all GPUs in a cluster as equally connected.
