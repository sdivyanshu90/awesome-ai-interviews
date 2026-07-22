### Q: Choose LoRA target modules, rank patterns, learning rates, and adapter composition strategies.
* **Difficulty:** Principal
* **Category:** Training
* **The 10-Second Pitch:** Select targets from task sensitivity and budget: attention projections influence routing; MLP projections carry much capacity; embeddings/output may matter for new tokens. Rank and LR should be ablated per module, not copied blindly.
* **The Deep Dive:** Start with Q/V or all attention and MLP linear layers depending on data/compute. Inspect gradient norms or low-rank update spectra from pilot/full tuning to allocate rank patterns. Higher rank increases capacity and memory but can overfit. LoRA often tolerates higher LR than full tuning because the base is frozen. Composition options include sequential training, weighted addition, concatenated rank, routing, or learned merging; updates can interfere because behavior is nonlinear even if matrices add.
A falsifiable check: sweep rank $r$ against $2r$ on identical data, targets, and schedule. If held-out target metrics do not improve and the learned update's singular values concentrate in the top few directions, capacity is not the bottleneck, and the budget should move to data quality or module coverage instead of rank.

* **Production Reality & Tradeoffs:** Adapter dropout, alpha, rank, batch, and schedule interact. Composition requires joint evaluation and safety checks. Embedding/tokenizer changes are not solved by ordinary projection adapters.
* **Red Flag:** Claiming Q/V-only LoRA is universally optimal.

