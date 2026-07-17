### Q: Serve, batch, cache, secure, merge, and roll back thousands of tenant adapters.
* **Difficulty:** Principal
* **Category:** LLMOps
* **The 10-Second Pitch:** Store signed, versioned adapters with explicit base-model compatibility; route by authorized tenant/task; isolate cache keys; batch only compatible adapters or use adapter-aware kernels; and evaluate every adapter for domain gain, retained capability, safety, and data leakage.
* **The Deep Dive:** Adapters may be merged into weights offline or applied dynamically. Composition can conflict because updates are not guaranteed additive in behavior. Registry metadata should bind base model digest, tokenizer, target modules, rank, training-data lineage, evaluation report, owner, and rollback state.
Bind every request to immutable base, adapter, tokenizer, template, and policy versions.
```text
identity -> authorized manifest -> signature/shape check
         -> adapter cache -> compatible batch -> inference
```
Keys include tenant/version; leases prevent in-flight eviction. Validate base hash, tensors, rank, dtype, and scale. Batch only compatible adapters or use heterogeneous-LoRA kernels. Tenant input must never choose arbitrary artifacts or poison shared cache state.

* **Production Reality & Tradeoffs:** Adapter loading can cause latency spikes and GPU fragmentation. Tenant isolation includes logs, prompts, retrieval, semantic cache, and telemetry—not only weights.
* **Red Flag:** Dynamically loading arbitrary uploaded adapter weights into a shared inference process.
