### Q: Define TTFT, inter-token latency, end-to-end latency, tokens/s, requests/s, goodput, and p99.
* **Difficulty:** Senior
* **Category:** Performance
* **The 10-Second Pitch:** TTFT measures arrival-to-first emitted token; ITL/TPOT measures gaps or average decode time; E2E spans full response; token/request throughput measure volume; goodput counts requests meeting SLO/quality; p99 captures tail.
* **The Deep Dive:** Timestamp queue admission, scheduling, tokenization, prefill, each decode, network flush, and completion. `tokens/s` must state input/output and per-user versus aggregate. Average TPOT can hide individual stalls; ITL distribution captures jitter. Requests/s is meaningless without length mix. Goodput excludes outputs missing latency/quality constraints. Percentiles require correct aggregation windows and trace clocks.
* **Production Reality & Tradeoffs:** Streaming lowers perceived latency but not completion. Client/network backpressure changes observed metrics. Report workload, concurrency, lengths, hardware, and warm state with benchmarks.
TTFT spans arrival through first streamed token; inter-token latency is the gap between emitted tokens; end-to-end ends at the last token. Tokens/s can mean per-request, aggregate output, or input+output and must be labeled. Goodput counts only work meeting an SLO. p99 needs a defined window and traffic mix.
$$
L_{E2E}\approx L_q+L_{prefill}+L_{first}+\sum_{t=2}^{T_{out}} ITL_t.
$$
Report distributions by prompt/output length and service class.

* **Red Flag:** Reporting peak tokens/s without latency or workload distribution.
