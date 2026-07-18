### Q: Compare static, dynamic, continuous/in-flight, iteration-level, and sequence batching.
* **Difficulty:** Senior
* **Category:** Scheduling
* **The 10-Second Pitch:** Static batches wait and run fixed sequences together; dynamic batching groups arrivals; continuous/iteration batching inserts/removes requests between decode steps; sequence batching preserves per-sequence state/order. Continuous batching improves utilization under variable lengths.
* **The Deep Dive:** Static padding wastes compute on short sequences. Dynamic request batching still suffers length divergence for full generation. Iteration-level scheduling forms a token batch each engine step from prefills and active decodes, admitting new requests as others finish. Sequence batching is a broader stateful serving concept used beyond LLMs. Scheduling policy chooses tokens, not merely requests, and must manage KV.
* **Production Reality & Tradeoffs:** Continuous batching raises throughput but can hurt TTFT/fairness under long prefills. Packed inputs complicate kernels and cancellation. Measure p99 and goodput.
Static batching waits for a fixed cohort and pads to the longest sequence. Dynamic batching forms a cohort within a delay window. Continuous/in-flight batching admits and removes sequences at iteration boundaries, reducing padding and head-of-line blocking. Iteration-level scheduling chooses token work each step. Compare under the same arrival and length distributions. Continuous batching still needs fairness: short decodes can be delayed by chunked prefills, and long generations can monopolize KV.

* **Red Flag:** Calling dynamic and continuous batching identical.
