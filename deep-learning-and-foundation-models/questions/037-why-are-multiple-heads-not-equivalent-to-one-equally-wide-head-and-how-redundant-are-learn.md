### Q: Why are multiple heads not equivalent to one equally wide head, and how redundant are learned heads?
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Each head has its own projected Q/K/V and independently normalized softmax map. One wide head has only one attention distribution, so it cannot generally reproduce multiple simultaneous routing patterns despite equal total width.
* **The Deep Dive:** Head $h$ computes $O_h=\operatorname{softmax}(Q_hK_h^T/\sqrt{d_h})V_h$. Concatenation and $W_O$ linearly mix results after $H$ separate nonlinear normalizations. A single $Hd_h$ head computes one score matrix and one softmax; no linear reparameterization generally converts one softmax into $H$ independent softmaxes. Multiple heads can attend to different positions/subspaces for the same query.

However learned heads are often redundant: head outputs or attention maps can correlate, some can be pruned with little immediate loss, and later layers compensate. Visual attention patterns do not establish causal function. Measure importance with head masking/ablation, output norm, gradient-based sensitivity, structured pruning followed by recovery training, and task/slice effects. Redundancy motivates MQA/GQA for K/V, head pruning, or talking-head mixing, but important rare-task heads may be missed by average benchmarks.
* **Production Reality & Tradeoffs:** More heads shrink $d_h$ at fixed $D$, potentially harming per-head capacity and kernel efficiency. Too few heads reduce routing diversity. Head importance is context/layer/task dependent, and pruning changes distributions.
* **Red Flag:** Claiming every head learns a human-interpretable relation, or that equal parameter count makes one wide head mathematically equivalent.

