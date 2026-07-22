### Q: Explain GraphRAG entity extraction, entity resolution, relation provenance, community detection, and summarization.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** GraphRAG converts passages into provenance-linked entities/relations, resolves duplicate identities, analyzes graph neighborhoods/communities, and retrieves paths or summaries. Every derived edge and summary must remain traceable to source spans.
* **The Deep Dive:** Extraction emits typed entity mentions and relation claims with confidence and source offsets. Resolution combines aliases, attributes, embeddings, and constraints while avoiding destructive merges. Edges may be temporal, directional, or contradictory. Community algorithms group densely related nodes; summaries compress themes for corpus-level questions. Query processing identifies seed entities, traverses constrained paths, and returns underlying evidence, not only generated summaries.

Entity resolution is a probabilistic record-linkage pipeline: blocking proposes candidate pairs, features compare names, attributes, and context, and hard constraints veto impossible merges. Preserve the underlying mention nodes so an uncertain merge can be reversed later. Edges should additionally record extractor version, and community summaries are derived caches that must be invalidated when membership, ACLs, or underlying evidence change.
* **Production Reality & Tradeoffs:** Graph construction is expensive and error-amplifying. Updates/deletes and ACL propagation must reach mentions, resolved entities, edges, communities, and caches. Evaluate relation/path correctness separately.
* **Red Flag:** Treating LLM-extracted graph facts as authoritative truth.
