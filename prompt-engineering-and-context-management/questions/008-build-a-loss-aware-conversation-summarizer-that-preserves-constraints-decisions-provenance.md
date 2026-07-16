### Q: Build a loss-aware conversation summarizer that preserves constraints, decisions, provenance, and uncertainty.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Summarize into a typed ledger that separates confirmed facts, user preferences, decisions, unresolved questions, constraints, and pending actions; attach source turn IDs and preserve uncertainty/conflict rather than inventing resolution.
* **The Deep Dive:** Define information obligations before prompting: high-authority instructions; identity/scope; commitments/decisions; exact numbers/dates/units; tool effects; open tasks; disputed/uncertain facts; and ephemeral chatter. Extract candidate atomic items with source turn IDs, then merge deterministically: exact confirmed state overrides older state only with an explicit correction; conflicts remain both with status; unverified model claims never become user facts. Maintain a short narrative only for conversational coherence, plus structured fields for execution.

```yaml
constraints: [{text, source_turn, authority}]
decisions: [{value, rationale, source_turn, status}]
open_questions: [...]
pending_actions: [{tool, args_digest, approval_state}]
uncertain_claims: [{claim, competing_evidence}]
summary_version: ...
```

Before replacing raw turns, run coverage checks against obligations and optionally keep cited source snippets. Compaction is immutable/versioned; corrections and deletion propagate. Evaluate through seeded long conversations where dropping one constraint causes observable failure, plus contradiction/numeric/entity perturbations.
* **Production Reality & Tradeoffs:** More retained detail reduces savings; automated faithfulness judges share model errors. High-risk commitments should remain structured/raw, not summary-only. Re-summarization can compound loss, so summarize from prior ledger plus new raw turns with periodic source reconstruction.
* **Red Flag:** Asking “summarize the conversation” and replacing authoritative history with unverifiable prose.

