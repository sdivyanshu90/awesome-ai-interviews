### Q: Design cache eviction and reuse under long context, branching, speculative decoding, and cancellation.
* **Difficulty:** Principal
* **Category:** Systems
* **The 10-Second Pitch:** Track block ownership/reference counts and recompute value; retain hot shared prefixes, evict unreferenced low-value blocks, copy-on-write branches, and release cancelled request state immediately.
* **The Deep Dive:** Each block metadata includes model/version/scope, prefix hash, refcount, last access, priority, size, transfer/recompute cost, and expiry. Long-context active blocks are pinned unless sliding attention permits eviction. Beam/speculative branches share committed prefix; rejected draft blocks are freed. Cancellation is idempotent and propagates across ranks/connectors. Policy may offload rather than evict high-reuse prefixes.
* **Production Reality & Tradeoffs:** LRU alone ignores size/cost/priority. Reference leaks cause OOM; premature free corrupts outputs. Test races among completion, cancellation, eviction, and branch merge.
Eviction should consider recompute cost, recency, prefix fan-out, block count, tenant quota, and deadline—not LRU alone. Pin active sequence blocks; reference-count shared prefixes; use copy-on-write after divergence; free speculative branches and cancelled requests promptly. Long contexts can evict many valuable short prefixes, so partition budgets or charge by blocks. Validate allocator invariants under concurrent allocate/fork/free/evict and expose internal fragmentation, failed allocations, and orphaned blocks.

* **Red Flag:** Treating KV blocks like independent HTTP response objects.
