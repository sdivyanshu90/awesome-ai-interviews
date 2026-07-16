### Q: Derive speculative decoding acceptance and explain draft-model speedup limits.
* **Difficulty:** Senior
* **Category:** Inference
* **The 10-Second Pitch:** A cheap draft proposes $\gamma$ tokens; one target pass scores them in parallel, accepts each prefix token with probability $\min(1,p/q)$, samples a corrected rejection token, and preserves the target distribution. Speedup depends on acceptance, target parallel efficiency, and draft/verification/rollback cost.
* **The Deep Dive:** Let draft distribution $q$ propose $x_1,\ldots,x_\gamma$. Target computes $p_i(\cdot)$ for all proposed-prefix positions. Token $x_i$ is accepted with probability $\alpha_i=\min(1,p_i(x_i)/q_i(x_i))$ until first rejection. At rejection sample from residual distribution proportional to $[p_i-q_i]_+$; if all accept, sample one extra target token. This rejection-correction yields exact samples from $p$ under matching decoding transforms.

If independent average acceptance is $a$, expected accepted draft prefix is $\sum_{i=1}^{\gamma}a^i$ (plus target token conventions). Approximate speedup compares tokens advanced per cycle with target verification time plus draft and orchestration. Larger $\gamma$ increases possible advance but wastes work after early rejection. A draft with mismatched tokenizer, temperature, top-p, or context gives low acceptance or invalid exactness.

```text
draft γ serial-cheap steps -> target one batched verification -> accept prefix / correct rejection -> repeat
```
* **Production Reality & Tradeoffs:** Verification can be slower than ordinary decode at small batches or unsupported shapes; draft consumes GPU/memory and may reduce batching. Measure p99 ITL/throughput, acceptance by domain/position, and scheduler effects.
* **Red Flag:** Saying speculative decoding changes outputs approximately, or estimating speedup as exactly $\gamma$.

