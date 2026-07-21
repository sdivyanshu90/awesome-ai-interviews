### Q: Explain provider prompt-caching mechanics and economics, and compute a break-even for a cached system prompt.
* **Difficulty:** Senior
* **Category:** Performance
* **The 10-Second Pitch:** Provider caches store the prefill (KV state) of an exact token prefix — marked with explicit breakpoints on Anthropic, matched automatically as longest-prefix on OpenAI — above a roughly 1024-token minimum, in TTL tiers from about five minutes to an hour. Writes carry a premium and reads a large discount, so break-even reduces to a hit-rate inequality: with a 1.25x write premium and a 10x read discount, caching pays above roughly a 22 percent hit rate.
* **The Deep Dive:** Mechanics. The cache is keyed on the model plus the exact serialized prefix: tokens, tool schemas, and any setting that changes prefill (enabling extended thinking, reordering tools) all participate, so one changed byte upstream invalidates everything after it. Anthropic uses explicit `cache_control` breakpoints (up to four) with a minimum cacheable prefix on the order of 1024 tokens (higher for the small fast models), a default five-minute TTL refreshed on each hit, and a paid one-hour tier. OpenAI caches automatically on the longest previously seen prefix above 1024 tokens, growing in 128-token steps, with no write premium and a substantial read discount, evicting entries after minutes of disuse. Both demand that dynamic content sit strictly after the stable prefix — a timestamp or user name at the top of the system prompt drives hits to zero. Hits are also placement-dependent: providers route requests by prefix affinity to the machine holding the KV state, and on your side stable serialization (fixed JSON key order, byte-identical templates) is what makes prefixes exact.

  Economics as ratios. Take base input price 1 per token, write premium $w$, read price $r$, cached prefix of $S$ tokens, and hit rate $h$. Expected prefix cost per request is $S\,(h r + (1-h)\,w)$, versus $S$ uncached, so caching wins when

  $$
  h > \frac{w-1}{w-r}.
  $$

  With Anthropic's five-minute tier ($w=1.25$, $r=0.1$) the threshold is $0.25/1.15 \approx 0.217$; the one-hour tier ($w=2$) needs $h > 0.526$; with no write premium ($w=1$) any nonzero hit rate saves money. Worked example for a support bot with a 4,000-token system prompt plus 6,000 tokens of tool schemas:

  ```text
  prefix S = 10,000 tokens; relative prices: base 1.0, write 1.25x, read 0.1x
  100 requests, no caching:      100 x 10,000 x 1.0  = 1,000,000 units
  caching at h = 0.9:  10 writes x 10,000 x 1.25 = 125,000
                       90 reads  x 10,000 x 0.10 =  90,000  -> 215,000 units
  result: ~4.7x cheaper prefill; break-even hit rate (w-1)/(w-r) = 0.25/1.15 ~ 0.22
  ```

  Hits also cut time-to-first-token, since prefill latency scales with uncached prefix length; on 10k-plus prefixes the TTFT drop is usually the more visible win. Falsifiable test: the API usage object reports cache-read and cache-write token counts. Compute realized $h$ from a day of production traffic and plug it into the inequality; if measured $h$ falls below the threshold, "we enabled caching so we are saving money" is falsified for that workload — fix session routing, move breakpoints, switch TTL tier, or remove the breakpoints.
* **Production Reality & Tradeoffs:** Hit rate collapses off-peak when gaps exceed the TTL; agents with long tool executions may need the one-hour tier. Every prompt or tool-schema deploy busts the fleet's cache simultaneously — expect a cost and TTFT spike and stagger rollouts. Development traffic shows near-zero hits (prompts change constantly), so measure economics only in production. Caches are scoped per organization and per model version, so upgrades re-prefill everything.
* **Red Flag:** Placing dynamic content at the top of the system prompt, or claiming savings without reading the cache usage fields — at a low hit rate the write premium makes caching strictly more expensive than not caching.
