### Q: Implement numerically stable multi-head attention and test it against a framework reference.
* **Difficulty:** Senior
* **Category:** Coding
* **The 10-Second Pitch:** Use explicit head reshapes, scale before softmax, combine masks before a safe key-axis softmax, accumulate critical reductions wider, and compare forward/backward against a trusted reference across pathological shapes and masks.
* **The Deep Dive:** Project Q/K/V or accept them as $[B,H,T,d]$. Compute $S=QK^T/\sqrt d$ in a suitable accumulator. Combine causal, padding, document, and attention masks into allowed edges. For each query subtract maximum over valid keys, exponentiate allowed scores, divide by valid sum, and define all-masked rows as zero output—not NaN/uniform. Multiply probabilities by V, transpose/reshape, and apply $W_O$. For GQA map query heads to KV groups without physically repeating cache if possible.

Reference tests set dropout zero and identical weights, then compare outputs, input/weight gradients, and cache-vs-full logits under FP64/FP32 tolerances. Hand-compute a tiny one-head case. Include extreme logits, all equal scores, one valid key, fully masked query, left/right padding, packed boundaries, $T_q\ne T_k$, noncontiguous tensors, odd $d$, and causal cache offsets. Directional finite differences validate custom backward.

```text
scores [B,H,Tq,Tk] -> mask -> row max over Tk -> exp/sum over Tk -> probabilities -> @V
```
* **Production Reality & Tradeoffs:** A stable reference may be slower than FlashAttention. API boolean mask polarity and finite sentinels differ. Dropout needs reproducible RNG in recomputation. Validate supported dtypes with error budgets, not bit equality.
* **Red Flag:** Subtracting one global maximum, normalizing over heads/queries, or letting a fully masked row silently attend uniformly.

