### Q: Derive top-k MoE routing, router probabilities, expert capacity, and output aggregation.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** A router scores each token over experts, selects top-$k$, normalizes gates, dispatches within capacity, runs expert MLPs, and combines outputs. Sparse active compute scales capacity, but routing balance and all-to-all communication become first-class.
* **The Deep Dive:** For token state $x_t$, router logits $r_t=W_rx_t$ and probabilities $p_{te}=\operatorname{softmax}(r_t)_e$. Select $S_t=\operatorname{TopK}(p_t,k)$ and output

$$
y_t=\sum_{e\in S_t}\tilde p_{te}E_e(x_t),
$$

where gates may be renormalized over selected experts. With $N$ tokens, $E$ experts, and capacity factor $c$, per-expert capacity is roughly $C=\lceil cNk/E\rceil$ (conventions differ). Tokens beyond capacity are dropped, rerouted, or handled by a shared expert. Expert-parallel execution packs tokens by destination, all-to-all dispatches, runs grouped GEMMs, then reverses the exchange.

Load-balancing losses combine router probability mass and actual assignment fractions; router z-loss penalizes large log-sum-exp logits. Top-k is discrete, so gradients flow through selected gates/auxiliary terms, not ordinary derivatives of selection. Failure modes include expert collapse/hotspots, tiny expert batches, overflow, shared-expert domination, router saturation, and cross-rank skew.
* **Production Reality & Tradeoffs:** Total parameters determine memory/checkpoint cost; active experts determine most compute; all-to-all and imbalance determine throughput. Capacity padding wastes compute while token dropping hurts quality. Monitor per-expert tokens, probability, capacity/drop, entropy, gradients, latency, and domain specialization.
* **Red Flag:** Claiming MoE makes a trillion-parameter model cost the same as a small dense model while ignoring weights, routing, capacity, and network.

