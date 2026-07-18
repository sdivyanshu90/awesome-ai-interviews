# 9. LLMOps, Serving & Infrastructure

Forty-seven interview profiles covering inference mechanics, scheduling and KV memory, serving engines and kernel optimization, quantization and speculation, distributed GPU training, profiling, and production platform design. Every question has its own answer file.

[Back to the complete syllabus](../SYLLABUS.md)

## Inference mechanics, metrics, scheduling, and KV memory

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 001 | [Trace tokenization, prefill, decode, sampling, detokenization, and streaming through an inference request.](questions/001-trace-tokenization-prefill-decode-sampling-detokenization-and-streaming-through-an-inferen.md) | Mid | Inference |
| 002 | [Why is prefill compute-bound while decode is commonly memory-bandwidth-bound?](questions/002-why-is-prefill-compute-bound-while-decode-is-commonly-memory-bandwidth-bound.md) | Senior | Systems |
| 003 | [Derive KV-cache memory and arithmetic intensity for MHA, GQA, and MQA.](questions/003-derive-kv-cache-memory-and-arithmetic-intensity-for-mha-gqa-and-mqa.md) | Senior | Math |
| 004 | [Define TTFT, inter-token latency, end-to-end latency, tokens/s, requests/s, goodput, and p99.](questions/004-define-ttft-inter-token-latency-end-to-end-latency-tokens-s-requests-s-goodput-and-p99.md) | Senior | Performance |
| 005 | [Build a latency-throughput-capacity model for mixed prompt/output lengths and concurrency.](questions/005-build-a-latency-throughput-capacity-model-for-mixed-prompt-output-lengths-and-concurrency.md) | Principal | Capacity Planning |
| 006 | [Compare static, dynamic, continuous/in-flight, iteration-level, and sequence batching.](questions/006-compare-static-dynamic-continuous-in-flight-iteration-level-and-sequence-batching.md) | Senior | Scheduling |
| 007 | [Explain PagedAttention block allocation, fragmentation, copy-on-write, eviction, and prefix sharing.](questions/007-explain-pagedattention-block-allocation-fragmentation-copy-on-write-eviction-and-prefix-sh.md) | Principal | Systems |
| 008 | [Compare contiguous, paged, quantized, offloaded, distributed, and persistent KV caches.](questions/008-compare-contiguous-paged-quantized-offloaded-distributed-and-persistent-kv-caches.md) | Principal | Systems |
| 009 | [Explain chunked prefill, prefill prioritization, decode prioritization, fairness, and starvation.](questions/009-explain-chunked-prefill-prefill-prioritization-decode-prioritization-fairness-and-starvati.md) | Principal | Scheduling |
| 010 | [Design admission control using KV capacity, token budgets, deadlines, and service classes.](questions/010-design-admission-control-using-kv-capacity-token-budgets-deadlines-and-service-classes.md) | Principal | System Design |
| 011 | [Compare prefix caching, KV reuse, semantic caching, cache salting, and tenant isolation.](questions/011-compare-prefix-caching-kv-reuse-semantic-caching-cache-salting-and-tenant-isolation.md) | Principal | Caching |
| 012 | [Design cache eviction and reuse under long context, branching, speculative decoding, and cancellation.](questions/012-design-cache-eviction-and-reuse-under-long-context-branching-speculative-decoding-and-canc.md) | Principal | Systems |

## Serving engines, kernels, quantization, and speculation

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 013 | [Explain vLLM’s serving architecture, scheduler, PagedAttention, continuous batching, and constraints.](questions/013-explain-vllm-s-serving-architecture-scheduler-pagedattention-continuous-batching-and-const.md) | Senior | Serving |
| 014 | [Explain TensorRT-LLM compilation, plugins, executor, scheduler, KV manager, sampler, and in-flight batching.](questions/014-explain-tensorrt-llm-compilation-plugins-executor-scheduler-kv-manager-sampler-and-in-flig.md) | Senior | Serving |
| 015 | [Compare vLLM, TensorRT-LLM, TGI, SGLang, llama.cpp, Triton, and custom runtimes.](questions/015-compare-vllm-tensorrt-llm-tgi-sglang-llama-cpp-triton-and-custom-runtimes.md) | Senior | Serving |
| 016 | [Explain kernel fusion, FlashAttention, fused MLP/norm/sampling, and memory-layout effects.](questions/016-explain-kernel-fusion-flashattention-fused-mlp-norm-sampling-and-memory-layout-effects.md) | Senior | GPU |
| 017 | [Explain CUDA Graphs, shape capture, padding, host overhead, and graph hit rate.](questions/017-explain-cuda-graphs-shape-capture-padding-host-overhead-and-graph-hit-rate.md) | Senior | GPU |
| 018 | [Diagnose when compilation, custom kernels, or graph capture regress rather than improve performance.](questions/018-diagnose-when-compilation-custom-kernels-or-graph-capture-regress-rather-than-improve-perf.md) | Principal | Performance |
| 019 | [Compare eager execution, torch.compile, TensorRT engines, Triton kernels, and handwritten CUDA.](questions/019-compare-eager-execution-torch-compile-tensorrt-engines-triton-kernels-and-handwritten-cuda.md) | Principal | GPU |
| 020 | [Compare weight-only, weight-activation, KV-cache, and mixed-precision quantization.](questions/020-compare-weight-only-weight-activation-kv-cache-and-mixed-precision-quantization.md) | Senior | Quantization |
| 021 | [Choose calibration data and detect layer, token, outlier, long-context, and tool-use regressions.](questions/021-choose-calibration-data-and-detect-layer-token-outlier-long-context-and-tool-use-regressio.md) | Principal | Quantization |
| 022 | [Derive speculative decoding verification and expected speedup from acceptance rate and draft cost.](questions/022-derive-speculative-decoding-verification-and-expected-speedup-from-acceptance-rate-and-dra.md) | Senior | Math |
| 023 | [Compare draft models, prompt lookup, Medusa-style heads, tree speculation, and disaggregated speculation.](questions/023-compare-draft-models-prompt-lookup-medusa-style-heads-tree-speculation-and-disaggregated-s.md) | Principal | Inference |
| 024 | [Explain why quantization or speculation can reduce throughput on the wrong hardware/workload.](questions/024-explain-why-quantization-or-speculation-can-reduce-throughput-on-the-wrong-hardware-worklo.md) | Principal | Performance |

## Distributed training and GPU fundamentals

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 025 | [Compare data, tensor, pipeline, sequence/context, expert, and optimizer-state parallelism.](questions/025-compare-data-tensor-pipeline-sequence-context-expert-and-optimizer-state-parallelism.md) | Senior | Distributed Training |
| 026 | [Compare all-reduce, reduce-scatter, all-gather, broadcast, and all-to-all communication.](questions/026-compare-all-reduce-reduce-scatter-all-gather-broadcast-and-all-to-all-communication.md) | Senior | Distributed Systems |
| 027 | [Design 1D/2D/3D/4D parallelism for dense and MoE models across nodes.](questions/027-design-1d-2d-3d-4d-parallelism-for-dense-and-moe-models-across-nodes.md) | Principal | Distributed Training |
| 028 | [Explain DDP, FSDP, ZeRO stages, sharded optimizers, and activation checkpointing.](questions/028-explain-ddp-fsdp-zero-stages-sharded-optimizers-and-activation-checkpointing.md) | Senior | Distributed Training |
| 029 | [Compare CPU/NVMe offload, recomputation, microbatching, and pipeline schedules.](questions/029-compare-cpu-nvme-offload-recomputation-microbatching-and-pipeline-schedules.md) | Principal | Distributed Training |
| 030 | [Diagnose stragglers, bubbles, imbalance, collective stalls, expert skew, and network congestion.](questions/030-diagnose-stragglers-bubbles-imbalance-collective-stalls-expert-skew-and-network-congestion.md) | Principal | Debugging |
| 031 | [Design fault-tolerant distributed checkpointing, restart, resharding, and elastic recovery.](questions/031-design-fault-tolerant-distributed-checkpointing-restart-resharding-and-elastic-recovery.md) | Principal | Reliability |
| 032 | [Explain GPU SMs, warps, occupancy, registers, shared memory, HBM, caches, and tensor cores.](questions/032-explain-gpu-sms-warps-occupancy-registers-shared-memory-hbm-caches-and-tensor-cores.md) | Senior | GPU |
| 033 | [Compare FP32, TF32, FP16, BF16, FP8, INT8, and INT4 numerical/hardware behavior.](questions/033-compare-fp32-tf32-fp16-bf16-fp8-int8-and-int4-numerical-hardware-behavior.md) | Senior | Numerics |
| 034 | [Use roofline analysis to distinguish compute, memory, communication, and launch-bound workloads.](questions/034-use-roofline-analysis-to-distinguish-compute-memory-communication-and-launch-bound-workloa.md) | Principal | Performance |
| 035 | [Profile with framework traces, Nsight Systems, Nsight Compute, NCCL telemetry, and GPU counters.](questions/035-profile-with-framework-traces-nsight-systems-nsight-compute-nccl-telemetry-and-gpu-counter.md) | Principal | Performance |
| 036 | [Diagnose low utilization, intermittent OOM, allocator fragmentation, thermal throttling, and tail spikes.](questions/036-diagnose-low-utilization-intermittent-oom-allocator-fragmentation-thermal-throttling-and-t.md) | Principal | Debugging |
| 037 | [Compare PCIe, NVLink, NVSwitch, InfiniBand, RoCE, Ethernet, and topology-aware placement.](questions/037-compare-pcie-nvlink-nvswitch-infiniband-roce-ethernet-and-topology-aware-placement.md) | Senior | Infrastructure |

## Production serving platforms

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 038 | [Design model loading, sharded weights, cold starts, warm pools, weight streaming, and lazy initialization.](questions/038-design-model-loading-sharded-weights-cold-starts-warm-pools-weight-streaming-and-lazy-init.md) | Principal | Serving |
| 039 | [Design autoscaling, queueing, rate limits, quotas, backpressure, shedding, and circuit breakers.](questions/039-design-autoscaling-queueing-rate-limits-quotas-backpressure-shedding-and-circuit-breakers.md) | Principal | Platform |
| 040 | [Serve heterogeneous chat, long-context, embedding, batch, multimodal, and agent workloads.](questions/040-serve-heterogeneous-chat-long-context-embedding-batch-multimodal-and-agent-workloads.md) | Principal | Platform |
| 041 | [Compare colocated versus disaggregated prefill/decode and KV transfer architectures.](questions/041-compare-colocated-versus-disaggregated-prefill-decode-and-kv-transfer-architectures.md) | Principal | Architecture |
| 042 | [Design multi-region serving with routing, residency, failover, replicated weights, and degraded modes.](questions/042-design-multi-region-serving-with-routing-residency-failover-replicated-weights-and-degrade.md) | Principal | System Design |
| 043 | [Build canary, shadow, A/B, rollback, compatibility, and model/prompt/index migration processes.](questions/043-build-canary-shadow-a-b-rollback-compatibility-and-model-prompt-index-migration-processes.md) | Principal | LLMOps |
| 044 | [Monitor quality, latency, saturation, drift, safety, cache efficiency, failures, and cost without leaking data.](questions/044-monitor-quality-latency-saturation-drift-safety-cache-efficiency-failures-and-cost-without.md) | Principal | Observability |
| 045 | [Design a distributed GPU inference platform with multi-tenant quotas and strict p99 SLOs.](questions/045-design-a-distributed-gpu-inference-platform-with-multi-tenant-quotas-and-strict-p99-slos.md) | Principal | System Design |
| 046 | [Design a trillion-parameter MoE training platform and identify its dominant failure modes.](questions/046-design-a-trillion-parameter-moe-training-platform-and-identify-its-dominant-failure-modes.md) | Principal | System Design |
| 047 | [Implement a continuous-batching scheduler and paged KV allocator simulator.](questions/047-implement-a-continuous-batching-scheduler-and-paged-kv-allocator-simulator.md) | Mid | Coding |

## Primary references

- [vLLM documentation](https://docs.vllm.ai/)
- [TensorRT-LLM documentation](https://docs.nvidia.com/tensorrt-llm/)
- [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/)
- [NCCL documentation](https://docs.nvidia.com/deeplearning/nccl/)
- [DeepSpeed ZeRO](https://www.deepspeed.ai/tutorials/zero/)
- [PyTorch FSDP](https://docs.pytorch.org/docs/stable/fsdp.html)
