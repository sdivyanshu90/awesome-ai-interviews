### Q: Design agentic retrieval: search as a tool, iterative query refinement, stopping criteria, and cost control versus single-shot RAG.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** Expose search as a tool the model calls in a plan-search-read-assess loop, refine follow-up queries against observed evidence, stop on coverage, marginal gain, or budget, and route so the loop runs only where cost-matched single-shot retrieval demonstrably fails.
* **The Deep Dive:** The tool contract is a function taking a query string, optional filters, and $k$, returning ranked spans with stable IDs, scores, and provenance. Loop state holds the original question, a working list of required sub-claims, evidence gathered so far, and the set of queries already issued. The decisive difference from static multi-query expansion is conditioning: the agent reads hop-one results, extracts the bridging entity or notices a coverage gap, and issues a hop-two query that no offline decomposition could have written because it depends on retrieved content. Upfront decomposition still helps for questions with visible structure; opportunistic refinement handles the rest.

  Stopping needs three independent rules because any one alone fails. Coverage: maintain an explicit checklist of sub-claims and stop when each is supported by at least one span, else abstain. Marginal gain: track the novelty ratio, the fraction of round-$t$ results not seen in rounds $1$ through $t-1$; stop when it falls below roughly $0.2$, since further rounds mostly re-retrieve. Budget: hard caps such as four tool calls, $20\,000$ total tokens, and an eight-second deadline, enforced outside the model. Loop hygiene prevents thrash: reject a new query whose embedding similarity to a prior query exceeds $0.95$ and force a mutation (add constraints, drop filters, switch to lexical), and dedupe evidence by span ID before it enters context.

  ```mermaid
  flowchart TD
    Q[User question] --> P[Plan sub-claims]
    P --> S[Search tool call]
    S --> R[Read and dedupe spans]
    R --> A{Coverage met?}
    A -- yes --> G[Generate with citations]
    A -- no --> M{Novelty above floor and budget left?}
    M -- yes --> F[Refine query from evidence] --> S
    M -- no --> X[Abstain or answer partially]
  ```

  The economics are asymmetric. Single-shot RAG costs one embedding call, one ANN plus rerank pass, and one generation over roughly $3\,000$ context tokens in a single model turn of one to two seconds. A three-round agent re-reads accumulated context each round, so total tokens land around three to six times the single-shot cost even with prompt caching discounting re-read prefixes, and latency is serialized: three model turns of about two seconds each pushes p50 past six seconds before generation. The loop earns that premium on multi-hop and vague queries over sparse corpora; on simple lookups it adds cost and a new failure mode, oscillating reformulation.
* **Production Reality & Tradeoffs:** Route with a cheap complexity classifier so the loop handles perhaps 10 to 20 percent of traffic, degrade to single-shot under load, and log every tool call for offline replay. The falsifying experiment is a cost-matched ablation: give single-shot the same total retrieval budget, for example fused multi-query with $k=30$ plus a reranker, and compare on a stratified eval. If the agent wins only against a starved $k=5$ baseline and ties the cost-matched one, the observed gain was retrieval depth, not iterative reasoning, and a wider one-shot pipeline is the correct, cheaper design. Monitor loop-length and abstention distributions per tenant; a drifting mean loop length usually signals corpus or router drift, not user behavior.
* **Red Flag:** Declaring the agent superior against a small-$k$ single-shot baseline without cost-matching, or shipping a loop whose stopping logic lives only in the prompt, with no external call, token, or deadline caps, so one pathological query burns unbounded tokens.
