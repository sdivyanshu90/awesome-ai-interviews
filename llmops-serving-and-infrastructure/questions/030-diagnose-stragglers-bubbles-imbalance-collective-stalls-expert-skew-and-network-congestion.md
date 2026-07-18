### Q: Diagnose stragglers, bubbles, imbalance, collective stalls, expert skew, and network congestion.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Use synchronized distributed traces to find the earliest rank/stage divergence, then distinguish compute variance, pipeline idle, data imbalance, collective wait, expert routing, or topology congestion.
* **The Deep Dive:** Collect per-rank step timelines, kernel/collective durations, queue depths, bytes, link counters, CPU/data-loader time, and expert tokens. A rank waiting in NCCL may be victim, not cause; inspect predecessor compute. Pipeline diagrams reveal fill/drain/stage imbalance. Correlate all-to-all tails with router skew and network paths. Reproduce with synthetic data/collective tests and compare healthy nodes.
* **Production Reality & Tradeoffs:** Transient hardware/ECC/thermal/filesystem issues mimic software imbalance. Avoid averaging ranks. Automated quarantine and elastic restart require proven correctness.
Start with a synchronized distributed timeline. Compute imbalance appears as ranks entering collectives late; network congestion appears as long collective duration after aligned entry; pipeline bubbles are scheduled idle gaps; expert skew concentrates tokens and capacity drops on a subset of ranks. Correlate step, rank, microbatch, expert load, NCCL channel, link counters, and host stalls. Fix the earliest divergence, since later collective waiting is often a symptom rather than the cause.

* **Red Flag:** Blaming NCCL because waiting appears inside a collective.
