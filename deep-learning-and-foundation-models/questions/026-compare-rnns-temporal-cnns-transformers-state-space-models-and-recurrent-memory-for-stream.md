### Q: Compare RNNs, temporal CNNs, Transformers, state-space models, and recurrent memory for streaming.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** RNNs/SSMs offer fixed-size recurrent state and low per-step latency; temporal CNNs offer bounded receptive fields and parallelism; Transformers offer content-based global access but growing cache. Hybrids trade exact recall, state size, training parallelism, and streaming latency.
* **The Deep Dive:** RNNs update a nonlinear state sequentially, $O(1)$ state per layer but limited long-credit paths. Causal temporal CNNs train in parallel and use dilations for large finite receptive fields; streaming caches recent activations. Transformers train all tokens in parallel and retrieve content directly, but dense training is $O(T^2)$ and decode KV grows $O(T)$. State-space models implement linear/selective recurrent dynamics; convolutional forms parallelize training while recurrent scans stream with fixed state. Their compressed state may lose exact arbitrary past tokens. External/recurrent memory adds explicit read/write and can preserve selected facts at orchestration complexity.

| Family | Training parallel | Streaming state | Long-range access |
|---|---:|---:|---|
| RNN | low | fixed | compressed |
| TCN | high | window/layers | fixed receptive field |
| Transformer | high | growing KV | direct content |
| SSM | high via scan | fixed | compressed selective |
* **Production Reality & Tradeoffs:** Asymptotic complexity does not predict GPU speed; kernels and sequence length matter. Evaluate causal chunking, state reset, exact recall, latency, memory, and robustness to distribution shifts. Hybrid attention+SSM may reserve attention for precise retrieval.
* **Red Flag:** Claiming fixed-state models remember unlimited detail or that Transformers cannot stream because training uses full sequences.

