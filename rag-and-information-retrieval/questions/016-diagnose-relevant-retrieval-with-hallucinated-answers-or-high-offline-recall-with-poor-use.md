### Q: Diagnose relevant retrieval with hallucinated answers, or high offline recall with poor user outcomes.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Verify metric integrity, then trace whether gains add duplicate/distracting/stale/unauthorized evidence, exceed context/latency budgets, or target incomplete relevance labels while harming generation and UX.
* **The Deep Dive:** Paired-replay old/new on production-like queries. Confirm same corpus/version, relevance unit, $k$, duplicate handling, filters, and judged pool. Higher Recall@k may come from overlapping chunks of one document, easier head queries, or incomplete labels. Inspect answer-support recall for all claims/hops, context precision, source authority/freshness/conflicts, and whether relevant spans survive packing. More candidates/chunks can push critical evidence into the middle/truncation, increase prompt injection, TTFT, and generator distraction.

Join stage traces to satisfaction/task outcomes by intent, language, query length, source, latency, and answer style. Use oracle-context/generation ablations. Online decline may be novelty, slower response, more verbose citations, fewer abstentions, or a changed user population—not retrieval relevance. Read adjudicated discordant examples and run a focused A/B on candidate count/order/reranker/context.
* **Production Reality & Tradeoffs:** Satisfaction is selectively observed and delayed; offline judgments are incomplete. Do not discard Recall—add construct-valid support, freshness, latency, and outcome metrics. Avoid post-hoc slice fishing.
* **Red Flag:** Increasing $k$ again because recall improved, without inspecting context precision and generation behavior.

