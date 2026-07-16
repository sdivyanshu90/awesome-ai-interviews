### Q: Derive KV-cache memory as a function of layers, tokens, batch, KV heads, head width, and dtype.
* **Difficulty:** Senior
* **Category:** Systems
* **The 10-Second Pitch:** Autoregressive decoding caches each layer’s keys and values for every retained token. Bytes are $2LBTH_{kv}d_hs$, so context, concurrency, layer count, KV heads, head width, and dtype directly bound serving capacity.
* **The Deep Dive:** At one layer, every token stores a key and value of shape $[H_{kv},d_h]$. For $L$ layers, $B$ sequences, length $T$, and $s$ bytes per element,

$$
M_{KV}=2\,L\,B\,T\,H_{kv}\,d_h\,s.
$$

MHA uses $H_{kv}=H_q$; GQA shares each KV head among query groups; MQA uses one, reducing cache linearly. Example: $L=32$, $T=8192$, $H_{kv}=8$, $d_h=128$, BF16 gives $2\cdot32\cdot8192\cdot8\cdot128\cdot2\approx1$ GiB per sequence. Allocator block rounding, metadata, beams/speculation, and temporary buffers add overhead.

Without cache, each decode step recomputes prior K/V and makes generation quadratic in repeated prefix work. With cache, projections for old tokens are reused, but each step reads a growing cache, making decode memory-bandwidth-heavy. Prefix sharing/copy-on-write lowers duplicated storage; sliding-window attention caps $T$; quantized/offloaded caches trade quality/latency.
* **Production Reality & Tradeoffs:** KV competes with weights, workspaces, CUDA graphs, and fragmentation. Admission must reserve output growth, not only current prompt. Cache format depends on model, adapter, RoPE positions, parallel layout, and dtype; unsafe cross-tenant reuse leaks data.
* **Red Flag:** Estimating concurrency from parameter memory alone, or saying KV cache reduces the mathematical attention length.

