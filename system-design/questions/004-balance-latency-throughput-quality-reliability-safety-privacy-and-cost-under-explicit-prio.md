### Q: Balance latency, throughput, quality, reliability, safety, privacy, and cost under explicit priorities.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Turn priorities into hard constraints and Pareto choices: gate unacceptable safety/privacy/reliability risk, then optimize quality and cost within workload-specific latency budgets.
* **The Deep Dive:** Define decision owners and severity. Some dimensions are constraints—authorization, residency, severe safety effects, minimum availability, p99—rather than weights a quality gain can offset. Build a workload matrix: user-facing chat values TTFT/ITL; batch values throughput/cost; high-stakes advice values evidence/review. Measure candidate architectures at identical traffic and quality gates. Levers include model routing, retrieval depth, context compression, caching, quantization, batching, speculation, parallelism, fallback, and human review. Plot Pareto frontiers and total cost per successful task, including retries and errors. Allocate latency budgets by stage and error budgets by dependency. Document degraded modes and triggers.
* **Production Reality & Tradeoffs:** A scalar weighted score hides catastrophic dimensions and stakeholder disagreement. Optimizing utilization hurts tails; smaller models may increase retries. Revisit priorities when workload or regulation changes and show uncertainty rather than false precision.
* **Red Flag:** Saying “it depends” without a measurable objective, constraints, decision, and experiment that would change it.

