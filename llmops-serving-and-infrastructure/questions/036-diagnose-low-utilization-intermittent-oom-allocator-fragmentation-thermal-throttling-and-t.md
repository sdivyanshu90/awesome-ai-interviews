### Q: Diagnose low utilization, intermittent OOM, allocator fragmentation, thermal throttling, and tail spikes.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Correlate request/rank timelines, memory snapshots, allocator stats, clocks/thermals, and scheduler queues. Distinguish insufficient work, stalls, true capacity, fragmentation, hardware throttling, and heavy-tail inputs.
* **The Deep Dive:** Low utilization may be CPU/tokenizer/data/network, tiny batches, sync, or launch-bound. OOM may follow KV growth, temporary workspace, graph capture, adapter loads, or leaks. Compare reserved/allocated/largest free block; paged allocators reduce but don't eliminate fragmentation. Track GPU clocks/power/temp/ECC. Tail spikes correlate with long prefill, cache miss, compilation, GC, noisy neighbor, or fabric congestion.
* **Production Reality & Tradeoffs:** Monitoring averages hides intermittent failures. Automated mitigation may reroute, drain, lower clocks/batch, or reject requests. Preserve triggering traces.
Low utilization may be CPU/tokenizer starvation, tiny batches, synchronization, or memory stalls. Intermittent OOM may be KV growth, transient workspace, fragmentation, or overlapping loads. Thermal throttling appears as clock/power/temperature correlation; tail spikes may follow cache misses, compaction, noisy neighbors, or retry storms. Join allocator snapshots, request lengths, scheduler state, clocks, kernel traces, and host metrics by timestamp. Reproduce by workload slice before changing allocator knobs.

* **Red Flag:** Fixing every OOM by reducing batch globally.
