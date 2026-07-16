### Q: Explain lost-in-the-middle behavior and evidence placement strategies.
* **Difficulty:** Senior
* **Category:** Context Management
* **The 10-Second Pitch:** Models often use evidence at context boundaries better than middle positions because of training length/position distributions, recency/primacy, positional schemes, attention competition, and integration depth. Placement helps but retrieval and verification matter more.
* **The Deep Dive:** Diagnose with a position sweep: keep question/evidence/distractors identical, move the support across normalized positions and lengths, swap the answer value to prevent memorization, and measure answer plus citation. Vary distractor similarity/count, number of evidence hops, and whether instructions are displaced. Inspect actual serialization/truncation/masks; configured context is not effective use.

Placement strategies put stable high-authority instructions at the beginning, the immediate query and most critical evidence near the end, and group each evidence chunk with title/source/span. Deduplicate, order by relevance and logical dependency, use section summaries/indexes for navigation, and repeat only short essential constraints rather than whole documents. For many documents, retrieve/rerank/compress instead of stuffing; preserve source boundaries. Long-context continued training/position scaling may improve curves but does not eliminate distractor failure.

```text
[policy + task] [structured state] [ranked evidence blocks] [question + output contract]
```
* **Production Reality & Tradeoffs:** Moving evidence can bias which source wins and dynamic ordering harms prefix caching. Repetition costs tokens and can overweight claims. Evaluate natural cross-document reasoning and p99 TTFT/KV, not synthetic needles alone.
* **Red Flag:** Putting all important content first or last without position-sweep evidence, or assuming longer context fixes retrieval.

