### Q: Implement a continuous-batching scheduler and paged KV allocator simulator.
* **Difficulty:** Mid
* **Category:** Coding
* **The 10-Second Pitch:** Simulate request arrivals, prefill/decode token budgets, fixed KV blocks, admission, cancellation, and scheduling policies; report TTFT, ITL, goodput, utilization, fragmentation, and starvation under deterministic tests.
* **The Deep Dive:** Use a deterministic discrete-event simulator. `Request` stores ID, arrival, prompt length/progress, generated/max output, priority/deadline, state, last-token time, and logical block table. `Allocator` owns fixed physical blocks, free list, refcounts, and optional immutable prefix index. Allocation occurs when a token crosses a block boundary; branching increments references and a write to shared tail invokes copy-on-write; finish/cancel decrements every reference exactly once. Admission reserves or probabilistically budgets worst-case KV separately from scheduling.

```text
on each scheduling event:
  ingest_arrivals_and_cancellations(now)
  reclaim_finished_blocks()
  candidates = policy.rank(waiting_prefills, active_decodes, deadlines, age)
  batch = pack(candidates, token_budget, sequence_budget, available_KV_blocks)
  duration = cost_model(prefill_tokens_by_shape, decode_sequences, cache_state)
  advance(batch, duration)       # prefill chunks or one/more decode iterations
  emit_tokens_and_metrics()
  schedule_next_event()
```

  Separate mechanism from policy: allocator correctness must not depend on FIFO, and the cost model must not choose requests. Model prefill cost by chunk/shape and decode cost by active batch and context distribution; constant cost per token hides the very trade-off being studied. Record TTFT, per-token latency, completion latency, deadline goodput, queue time, token throughput, KV occupancy, tail waste, shared hits, preemptions, and starvation by class. Invariants include nonnegative refcounts, unique ownership of writable blocks, no leaked blocks, logical-token capacity, deterministic tie breaks, and conservation of blocks.
* **Production Reality & Tradeoffs:** Calibrate cost distributions from traces on the target model/hardware and sensitivity-test burstiness, length correlation, cancellation, and estimation error. Unit tests cover exact-boundary allocation, final-block waste, OOM admission, shared-tail COW, concurrent cancellation, prefix eviction, deadline ties, and starvation; randomized property tests check allocator invariants. The simulator predicts relative policy behavior, not kernel-level absolute latency unless its cost model is measured and continuously revalidated.
* **Red Flag:** Implementing FIFO requests without token-level state or KV capacity.
