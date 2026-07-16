### Q: Compare truncation, sliding windows, summaries, hierarchical context, retrieval, and structured state.
* **Difficulty:** Senior
* **Category:** System Design
* **The 10-Second Pitch:** Manage context by information value and invariants: keep authoritative instructions/current state verbatim, retrieve relevant evidence, summarize lossy history with provenance, and truncate only low-value content under explicit budgets.
* **The Deep Dive:** Truncation is deterministic and cheap but can remove constraints/evidence; never use blind left truncation that drops policy. Sliding windows preserve recency and suit local conversation/streaming but forget long-range commitments. Summaries compress broadly yet introduce omissions/hallucinations and need source spans/version plus correction. Hierarchical context keeps recent raw turns, rolling summaries, topic/episode summaries, and long-term index. Retrieval selects query-relevant memory/documents but can miss globally important facts and is vulnerable to ACL/freshness/injection. Structured state stores exact entities, decisions, pending actions, preferences, and tool results with typed fields rather than relying on prose memory.

```text
fixed high-authority policy + typed current state + recent raw turns
                    + retrieved evidence + provenance-rich summaries
                    <= token budget with reserved output/tool space
```

Assign each item priority, authority, freshness, sensitivity, dependency, token cost, and fallback. Deduplicate, preserve document boundaries, and emit a trace of what was omitted. Evaluate answer quality under counterfactual removals and long conversations.
* **Production Reality & Tradeoffs:** Larger windows raise prefill/KV cost and distractors; summarization loses detail; retrieval adds latency/failure. Compaction is a state migration and must be versioned/replayable. Tenant scope and deletion apply to every cache/memory.
* **Red Flag:** Keeping the most recent tokens only, or treating a generated summary as authoritative state.

