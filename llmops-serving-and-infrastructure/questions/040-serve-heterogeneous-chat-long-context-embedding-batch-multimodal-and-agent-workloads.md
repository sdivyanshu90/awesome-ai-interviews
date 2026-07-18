### Q: Serve heterogeneous chat, long-context, embedding, batch, multimodal, and agent workloads.
* **Difficulty:** Principal
* **Category:** Platform
* **The 10-Second Pitch:** Classify workloads and isolate incompatible resource/SLO profiles into pools or schedulers while sharing gateways, identity, quotas, observability, and routing.
* **The Deep Dive:** Chat decode needs low ITL; long context needs prefill/KV capacity; embeddings favor large batches; offline batch maximizes throughput; multimodal adds encoders/tokens; agents create bursty recursive calls/tools. Route by model, length, modality, deadline, priority, and tenant. Separate queues/pools prevent head-of-line blocking; opportunistically fill spare capacity with preemptible batch. Admission predicts full task budgets for agents.
* **Production Reality & Tradeoffs:** Too many pools waste capacity; one pool harms tails. Use measured interference and dynamic routing. Cross-workload caches/privacy require scope.
Separate pools or scheduling classes for embeddings, interactive short chat, long-context prefill, batch generation, multimodal encoders, and agent bursts because their compute, memory, deadline, and batching shapes conflict. A router estimates token/media work and assigns compatible hardware/runtime. Shared capacity can absorb variance, but reservations prevent one class from consuming KV or starving decode. Report SLO/goodput/cost per class and define degradation: cap context/output, defer batch, or reject—never silently change semantics.

* **Red Flag:** Putting all requests behind one FIFO GPU queue.
