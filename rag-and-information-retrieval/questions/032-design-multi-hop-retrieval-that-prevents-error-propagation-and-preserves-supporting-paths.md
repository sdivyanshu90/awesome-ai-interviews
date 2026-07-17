### Q: Design multi-hop retrieval that prevents error propagation and preserves supporting paths.
* **Difficulty:** Principal
* **Category:** RAG
* **The 10-Second Pitch:** Decompose only when evidence truly spans hops, verify each intermediate entity/claim, carry provenance through the chain, and allow backtracking or abstention when a hop is weak.
* **The Deep Dive:** Represent a plan as subquestions with expected answer types and dependencies. Retrieve/rerank for hop one, extract candidates with citations, then form hop-two queries from validated entities—not free-form summaries. Keep multiple hypotheses to avoid early commitment, constrain graph/metadata relations, and score the complete evidence path. Final generation receives path IDs and source spans; a verifier checks each link and overall entailment.
* **Production Reality & Tradeoffs:** Branching increases latency and noise. Cached intermediate facts need version/ACL scope. Evaluate hop recall and path faithfulness, not only final answer exact match.
Score a path from per-hop retrieval, entity-link confidence, relation compatibility, and final support; multiplying raw confidences often over-penalizes long paths, while summing ignores weakest links, so calibrate on path labels. Beam search keeps alternative intermediates and permits backtracking. Stop if no path satisfies minimum evidence rather than filling the missing hop with parametric knowledge. Evaluate oracle decomposition, each hop’s recall, full path validity, and final answer separately.

* **Red Flag:** Letting an unsupported first-hop answer become the next query as if it were fact.
