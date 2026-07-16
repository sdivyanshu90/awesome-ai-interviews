### Q: Allocate a token budget across instructions, examples, conversation, retrieval, tools, and output.
* **Difficulty:** Senior
* **Category:** Context Management
* **The 10-Second Pitch:** Reserve output and hard system/tool overhead first, preserve authoritative state and evidence obligations, then optimize examples/history/retrieval by marginal utility per token with explicit truncation and fallback.
* **The Deep Dive:** Let maximum context be $C$. Enforce

$$
C_{policy}+C_{tools}+C_{state}+C_{recent}+C_{examples}+C_{retrieval}+C_{output}+C_{margin}\le C.
$$

Reserve maximum output/tool-call tokens and safety margin before input packing. Policy and active tool schemas are hard constraints; typed current state/commitments outrank prose history. Recent raw turns preserve interaction; summaries cover older history with provenance. Select demonstrations for coverage/relevance and retrieval chunks for claim support/diversity, deduplicating overlaps. Score candidates by priority, authority, freshness, dependency, expected utility, and tokens; use per-section caps so one long document cannot evict everything.

If over budget, drop low-value examples/chatter, compress/retrieve selectively, or ask a clarifying question; never silently truncate policy, action arguments, or evidence mid-record. Log selected/omitted item IDs and token counts. Evaluate marginal ablations and quality/latency versus budget on real length distributions.
* **Production Reality & Tradeoffs:** Tokenizer/model changes alter counts. More evidence can hurt through distractors and TTFT/KV. Summaries are lossy, while retrieval can miss. Dynamic packing reduces prefix-cache hits. Budget p99, not average.
* **Red Flag:** Sending content then relying on provider truncation, or allocating the entire window to input with no output reserve.

