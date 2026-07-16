### Q: What causes attention sinks, lost-in-the-middle behavior, and long-context retrieval degradation?
* **Difficulty:** Principal
* **Category:** Foundation Models
* **The 10-Second Pitch:** Softmax must allocate mass and early/structural tokens can become stable sinks; long training distributions, position encoding, distractor competition, context construction, and limited effective computation create position-dependent evidence use even within nominal context.
* **The Deep Dive:** Attention sinks are tokens—often initial/special tokens—that receive attention despite little semantic content, providing a stable place for excess mass or residual-state calibration. Removing them from a cache can sharply alter outputs. Lost-in-the-middle describes lower use of relevant evidence away from recent/initial positions. Causes include training examples concentrated at short lengths or boundary positions; RoPE/relative bias phase or recency; more competing keys/distractors; softmax dilution; limited depth for multi-hop integration; truncation/chunk ordering; and evaluation artifacts.

Diagnose with controlled counterfactuals: place identical evidence at every position/length, swap answer values, vary distractor similarity/count, test one versus multiple hops, inspect actual serialized tokens/masks, and compare retrieval success to answer use. Attention visualization alone is not causal; ablate/move evidence and patch activations/cache. Track source citation and confidence.

Mitigations include long-context continued training with position-balanced data, RoPE scaling, retrieval/context compression, chunk ordering, global/memory tokens, recurrent summaries, attention-sink preservation, and architecture changes.
* **Production Reality & Tradeoffs:** Longer context raises TTFT/KV/cost and can worsen distractor susceptibility. Synthetic needles overestimate natural reasoning. Retrieval may beat raw context for freshness/attribution but can miss evidence.
* **Red Flag:** Assuming evidence is usable because it fits under the advertised token limit, or diagnosing solely from heatmaps.

