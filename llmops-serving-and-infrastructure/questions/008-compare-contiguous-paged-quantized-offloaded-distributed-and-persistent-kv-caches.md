### Q: Compare contiguous, paged, quantized, offloaded, distributed, and persistent KV caches.
* **Difficulty:** Principal
* **Category:** Systems
* **The 10-Second Pitch:** Contiguous cache is simple but overallocates; paged allocates blocks on demand; quantized reduces bytes; offloaded moves cold blocks; distributed shards cache; persistent/external cache reuses prefixes across workers. Each trades latency, complexity, and fidelity.
* **The Deep Dive:** Contiguous tensors optimize addressing but reserve `[max batch,max length]`. Paged pools improve utilization. FP8/INT8/INT4 cache needs scale metadata and dequant kernels. CPU/NVMe/network offload adds bandwidth/latency and needs prefetch. Tensor/context parallelism determines which KV heads/tokens reside per rank. External connectors support prefill/decode disaggregation and cross-request reuse, requiring model/token exactness and ownership metadata.
* **Production Reality & Tradeoffs:** Offload may be slower than recompute. Quantization errors accumulate in long attention. Persistent caches create privacy/invalidation risks. Benchmark hit rate and transfer tails.
A contiguous cache reserves each sequence’s maximum or grows/copies buffers; paging maps logical token blocks to noncontiguous physical blocks; quantization reduces bytes with quality/kernel cost; offload trades HBM for PCIe/NVLink latency; distributed KV enables sharded/disaggregated execution; persistent prefix KV reuses immutable prefixes. Capacity is approximately layers × KV heads × head dimension × two tensors × bytes × cached tokens. Include block fragmentation and copy-on-write branches.

* **Red Flag:** Treating every cache tier as transparent memory expansion.
