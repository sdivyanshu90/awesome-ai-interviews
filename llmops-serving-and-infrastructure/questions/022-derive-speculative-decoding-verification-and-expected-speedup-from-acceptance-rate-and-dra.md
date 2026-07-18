### Q: Derive speculative decoding verification and expected speedup from acceptance rate and draft cost.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** A draft proposes `k` tokens; target verifies them in one pass and accepts sequentially with a correction rule preserving target distribution. Speedup depends on accepted tokens per target pass minus draft/verification overhead.
* **The Deep Dive:** For draft `q` and target `p`, canonical rejection sampling accepts token with `min(1,p/q)` and on rejection samples from normalized positive residual `(p-q)_+`; all-accepted paths can add one target token. Expected accepted prefix for roughly independent acceptance `a` is a geometric sum. Wall time includes draft autoregression, target multi-token verification, sampling, KV management, and batch effects. Good draft maximizes acceptance per unit cost.
* **Production Reality & Tradeoffs:** At high continuous batch, target is already efficient and draft may reduce throughput. Distribution-correctness depends on exact algorithm; approximations need evaluation.
If the draft proposes $k$ tokens, the target evaluates them in parallel and accepts the longest prefix consistent with target sampling; on rejection, it samples a corrected token from the residual distribution, preserving the target distribution under the proper algorithm. Speed depends on accepted tokens per target pass versus draft, verification, and KV overhead. A useful approximation compares $(E[A]+1)$ emitted tokens with draft-plus-one-target cost. Measure acceptance by position and query class.

* **Red Flag:** Estimating speedup as exactly `k×`.
