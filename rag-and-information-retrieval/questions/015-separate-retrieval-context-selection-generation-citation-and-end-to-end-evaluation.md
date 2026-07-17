### Q: Separate retrieval, context-selection, generation, citation, and end-to-end evaluation.
* **Difficulty:** Senior
* **Category:** Debugging
* **The 10-Second Pitch:** Evaluate each boundary with its own oracle: corpus/index candidate recall, post-rerank/context support, generator faithfulness under gold context, citation entailment, and end-to-end task success. A single answer score cannot localize failure.
* **The Deep Dive:** Corpus/ingestion asks whether authoritative current evidence exists and is parsed/chunked correctly. Retrieval measures relevant/support-set recall and ranking. Context selection measures whether required spans survive dedup, parent expansion, token packing, ordering, and truncation. Generation receives oracle context to test claim correctness, instruction following, conflict handling, and abstention independent of retrieval. Citation evaluation maps atomic claims to exact supplied spans and checks completeness/entailment/source quality. End-to-end includes authorization, latency, cost, UX, and outcomes.

```text
corpus -> candidates -> reranked/context-packed evidence -> answer claims -> citations -> user outcome
  oracle     oracle                 gold-context test        entailment
```

Log immutable IDs/versions and run counterfactuals: replace retrieved context with gold; replace model answer with templated extraction; move evidence; inject distractors; remove one required hop. Relevant documents can still yield hallucination because the key span is truncated, conflicting text ignored, generator relies on prior, or citation points broadly.
* **Production Reality & Tradeoffs:** Stage labels are expensive and alternate evidence makes ground truth incomplete. Sample expert audits and propagate uncertainty. Optimizing one stage may hurt another (large chunks raise recall but dilute context).
* **Red Flag:** Calling every bad answer a retrieval problem or declaring grounding because a relevant document appeared somewhere in top-k.

