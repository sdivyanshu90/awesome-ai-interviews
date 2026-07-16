### Q: How should demonstrations be selected, ordered, balanced, and formatted?
* **Difficulty:** Senior
* **Category:** Prompt Design
* **The 10-Second Pitch:** Select examples for coverage and query relevance under a token budget, balance labels and failure modes, use one canonical schema, and treat order as a tested parameter because recency and local similarity bias predictions.
* **The Deep Dive:** Start from the target distribution and error taxonomy. Include boundary cases, rare labels, abstention/missing-data behavior, and confusing negatives; remove duplicates and any example whose rationale/label is uncertain. Static examples provide stable behavior/cache hits; retrieval-selected examples adapt to the query but add latency, nondeterminism, leakage, and prompt-injection risk. Selection methods include stratified coverage, embedding similarity, diversity/maximal-marginal relevance, learned utility, and leave-one-example-out ablation.

Use consistent separators, field names, output schema, and granularity. Keep examples isomorphic to the actual instruction: if reasoning is hidden, do not require the model to copy verbose rationales. Randomize/counterbalance order during evaluation; test class-first/last effects, nearest example last, and label verbalizers. Avoid a block where all one label occurs late, which confounds recency with class. Reserve output tokens and ensure truncation never cuts an example in half.
* **Production Reality & Tradeoffs:** More examples can hurt through distraction, contradiction, and cost. Similar examples may copy irrelevant details; diverse examples may dilute task identity. Version demonstrations as data, with provenance, privacy, contamination checks, and regression tests.
* **Red Flag:** Choosing examples by intuition and reporting one favorable ordering, or using the test query/answer to select demonstrations.

