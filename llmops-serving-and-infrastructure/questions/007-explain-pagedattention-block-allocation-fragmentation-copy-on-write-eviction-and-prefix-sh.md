### Q: Explain PagedAttention block allocation, fragmentation, copy-on-write, eviction, and prefix sharing.
* **Difficulty:** Principal
* **Category:** Systems
* **The 10-Second Pitch:** PagedAttention stores KV in fixed-size blocks referenced by per-sequence tables, allocating as tokens arrive. This avoids reserving max length, reduces external fragmentation, enables shared prefix blocks and copy-on-write branches.
* **The Deep Dive:** Divide each layer’s KV storage into physical blocks of $P$ token slots. Sequence $r$ owns a logical block table $BT_r[j]=physical_id$; token position $t$ resolves to block $BT_r[floor(t/P)]$ and offset $t mod P$. New blocks are allocated only as the sequence grows, so the allocator does not reserve maximum context. External fragmentation largely disappears because any free physical block can satisfy any logical block; internal waste is limited to unused positions in each sequence’s final block—less than $P$ token slots per sequence.

```text
sequence A logical blocks: [0] [1] [2]  ---> physical blocks: [7] [3] [9]
sequence B logical blocks: [0] [1]      --->                  [7] [3]  (shared prefix)
                                                                |
                                      B diverges: refcount>1 -> copy final shared block -> [5]
```

  The attention kernel receives block tables and gathers K/V in logical order; paging changes address translation, not attention mathematics or raw KV bytes. Prefix caching keys immutable full blocks by exact token/model/adapter/position/cache-policy identity. Beam and speculative branches increment reference counts; a write to a partially shared tail uses copy-on-write. Cancellation decrements references and returns blocks only at zero. Eviction must exclude pinned active blocks and weigh recency, tenant priority, recomputation cost, transfer cost, and prefix fan-out. Preemption may swap, recompute, or reject, but ownership transitions must be atomic.
* **Production Reality & Tradeoffs:** Smaller blocks lower tail waste and increase metadata/lookup overhead; larger blocks do the reverse. Noncontiguous layouts require specialized kernels and can reduce locality. Prefix reuse needs exact compatibility and tenant/policy scoping or salting to prevent timing and data leaks. Track free blocks, tail waste, shared-block hit rate, refcounts, preemption, and allocation failures. Define deterministic overload behavior—unbounded optimistic allocation turns OOM into a fleet-wide tail-latency event.
* **Red Flag:** Saying paging reduces the mathematical KV size.
