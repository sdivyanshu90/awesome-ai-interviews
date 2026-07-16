### Q: Explain load-balancing losses, expert collapse, token dropping, shared experts, and router z-loss.
* **Difficulty:** Principal
* **Category:** MoE
* **The 10-Second Pitch:** Sparse routers naturally concentrate on a few experts; auxiliary balance terms align probability mass and assignments, capacity bounds buffers, z-loss limits logit growth, and shared experts provide an always-available path. Each can distort specialization or waste compute.
* **The Deep Dive:** Let $p_{te}$ be router probability and $f_e$ fraction of selected token assignments. A common auxiliary loss is proportional to $E\sum_e \bar p_e f_e$ (normalization varies), discouraging simultaneous high probability/traffic concentration. Because top-k assignments are discrete, probability terms provide gradient signals even when counts do not. Router z-loss penalizes squared $\log\sum_e e^{r_{te}}$, controlling large logits/softmax saturation.

Expert collapse appears as low routing entropy, hot experts, cold experts with no gradients, capacity overflow, and domain overconcentration. Capacity factor reserves roughly $cNk/E$ slots; overflow tokens may drop, reroute, pad, or use shared/dense experts. Dropping silently changes model computation and can disproportionately affect domains. Shared experts guarantee common transformation and reduce loss from routing errors but add active compute and may dominate sparse experts.

Monitor per expert probability, tokens before/after capacity, drops, entropy, batch size, gradient/update norm, output norm, domain/language mix, and latency—not only auxiliary loss.
* **Production Reality & Tradeoffs:** Stronger balancing improves utilization but can force semantically wrong routing and reduce specialization. Higher capacity wastes memory/padding. Expert-choice or dropless routing changes communication/control complexity. Balance globally and per expert-parallel group.
* **Red Flag:** Tuning one auxiliary coefficient until token counts are equal and assuming quality/network balance is solved.

