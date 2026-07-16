### Q: Why does teacher forcing parallelize training while autoregressive inference remains sequential?
* **Difficulty:** Mid
* **Category:** Conceptual
* **The 10-Second Pitch:** Training knows the entire ground-truth prefix, so one causal forward pass scores every next-token conditional in parallel. Inference does not know token $t+1$ until sampling token $t$, creating an unavoidable dependency chain for ordinary autoregressive generation.
* **The Deep Dive:** With teacher forcing, input tokens $x_{1:T-1}$ are available and a triangular mask ensures hidden position $t$ uses only $x_{\le t}$. Matrix operations compute logits for all targets $x_{2:T}$ simultaneously. The factorization remains autoregressive; parallel computation does not reveal future labels because the mask blocks them.

During generation,

$$
x_t\sim p_\theta(\cdot\mid x_{<t}),
$$

and $x_t$ changes the next input, position, KV cache, stop conditions, and possibly tool/control state. Therefore decode has a serial outer loop even though each layer/head/GPU operation and the batch of sequences run in parallel. KV caching avoids recomputing prior K/V but cannot remove token dependency. Speculative decoding proposes several draft tokens and verifies them in parallel with the target, reducing target passes when acceptance is high; non-autoregressive models change the factorization/objective and quality trade-off.
* **Production Reality & Tradeoffs:** Prefill is parallel/compute-heavy; decode is iterative/bandwidth-heavy, so serving schedules them differently. Training–inference prefix mismatch contributes to exposure bias, but teacher forcing is still the exact MLE estimator for observed sequences.
* **Red Flag:** Saying causal masking makes training sequential, or claiming KV caching lets all future tokens be generated at once.

