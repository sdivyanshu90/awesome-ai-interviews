### Q: Design multi-hop retrieval that prevents error propagation and preserves supporting paths.
* **Difficulty:** Principal
* **Category:** RAG
* **The 10-Second Pitch:** Decompose only when evidence truly spans hops, verify each intermediate entity/claim, carry provenance through the chain, and allow backtracking or abstention when a hop is weak.
* **The Deep Dive:** Represent a plan as subquestions with expected answer types and dependencies. Retrieve/rerank for hop one, extract candidates with citations, then form hop-two queries from validated entities—not free-form summaries. Keep multiple hypotheses to avoid early commitment, constrain graph/metadata relations, and score the complete evidence path. Final generation receives path IDs and source spans; a verifier checks each link and overall entailment.

Score a path from per-hop retrieval strength, entity-link confidence, relation compatibility, and final answer support. Multiplying raw confidences over-penalizes long paths while summing ignores the weakest link, so calibrate the aggregation on labeled paths. Stop when no path meets a minimum evidence threshold instead of filling the missing hop from parametric memory, and use oracle decompositions in evaluation to separate planning failures from retrieval failures.
* **Production Reality & Tradeoffs:** Branching increases latency and noise. Cached intermediate facts need version/ACL scope. Evaluate hop recall and path faithfulness, not only final answer exact match.
* **Red Flag:** Letting an unsupported first-hop answer become the next query as if it were fact.
