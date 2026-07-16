### Q: Compare dense, local, sliding-window, dilated, block-sparse, linear, and recurrent attention.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** These patterns trade direct arbitrary token interaction for compute/state: dense is exact global $O(T^2)$; local/window/dilated/block-sparse restrict edges; linear attention factorizes kernels; recurrent attention compresses history into fixed state.
* **The Deep Dive:** Dense softmax attention gives every query every key and maximum path length one, but quadratic scores. Sliding-window attends the last/nearby $W$ tokens for $O(TW)$ and bounded KV, often adding global tokens or periodic full layers. Dilated patterns sample distant positions sparsely; stacked layers fill gaps but can miss precise evidence. Block-sparse defines hardware-friendly allowed block pairs (local, global, strided, document) and remains exact on those edges.

Linear attention replaces $\operatorname{softmax}(QK^T)$ with feature map $\phi(Q)\phi(K)^T$, reorders $(\phi(Q)(\phi(K)^TV))$, and maintains prefix statistics causally; normalization/positivity and approximation change behavior. Recurrent/state methods update compressed memory, achieving fixed per-token state but losing arbitrary exact retrieval unless state capacity/selection suffices.

| Pattern | Cost | Exact arbitrary past lookup? | State |
|---|---:|---:|---:|
| dense | $T^2$ | yes | growing |
| window | $TW$ | within window | bounded |
| block sparse | allowed blocks | pattern-dependent | pattern-dependent |
| linear/recurrent | near $T$ | no guarantee | fixed/compressed |
* **Production Reality & Tradeoffs:** Asymptotics hide kernel utilization and crossover lengths. Sparse patterns need layout-compatible kernels. Evaluate local quality, long retrieval positions, distractors, generation, and actual latency/memory.
* **Red Flag:** Calling any subquadratic method “the same attention but faster” or judging only perplexity at short lengths.

