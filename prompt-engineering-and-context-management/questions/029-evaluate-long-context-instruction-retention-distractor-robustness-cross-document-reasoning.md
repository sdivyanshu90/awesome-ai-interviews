### Q: Evaluate long-context instruction retention, distractor robustness, cross-document reasoning, and citation use.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Factor the evaluation: sweep length and position, move/negate instructions, vary distractor hardness, require labeled multi-document supports, and score answer/citations jointly with latency, KV, and truncation.
* **The Deep Dive:** Create controlled families, not isolated needles. Instruction retention: place constraints at different authority/positions, insert conflicting lower-authority text, and test each clause. Retrieval: place answer spans across lengths/positions with counterfactual values. Distractors progress from irrelevant to lexical matches, plausible contradictions, and prompt injection. Cross-document tasks require complementary facts, temporal ordering, or conflict resolution; label each necessary hop and include unanswerable cases. Citation evaluation checks completeness, entailment, source quality, and exact span—not marker count.

Run exact rendered context through the complete builder/model. Report curves over effective tokens and normalized position, joint-support recall, claim faithfulness, abstention, instruction violation, and citation accuracy. Measure tokenization, truncation, prefill TTFT, ITL, memory, and cost. Group related variants in bootstrap intervals and keep task families held out.

```text
length × evidence position × distractor hardness × required hops × instruction position
                         -> quality + citation + systems curves
```
* **Production Reality & Tradeoffs:** Synthetic patterns can be memorized/easier than real documents. LLM judges share long-context blind spots; validate with humans. Longer contexts change serving concurrency, so quality-only benchmarks are incomplete.
* **Red Flag:** Reporting one maximum-length needle score or answer accuracy without verifying required sources and instructions.

