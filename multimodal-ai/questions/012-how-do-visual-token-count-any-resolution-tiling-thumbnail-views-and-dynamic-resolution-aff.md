### Q: How do visual-token count, any-resolution tiling, thumbnail views, and dynamic resolution affect cost and accuracy?
* **Difficulty:** Principal
* **Category:** Systems
* **The 10-Second Pitch:** Visual tokens trade evidence resolution against prefill/attention cost. Any-resolution systems tile detail and add a global thumbnail; dynamic policies allocate more tiles only when the input/task needs them.
* **The Deep Dive:** If tile encoder emits `n` tokens and there are `k` tiles plus thumbnail, the LLM receives roughly `(k+1)n` visual tokens before resampling. Global thumbnail provides layout; tiles preserve text/small objects. A routing policy may inspect dimensions or low-res features to choose grid/resolution. Token pruning/resampling can compress redundant background. Spatial IDs and tile order preserve coordinates.
* **Production Reality & Tradeoffs:** Worst-case images drive p99 and KV memory. Dynamic resolution complicates batching and CUDA graphs. More tiles can duplicate objects and degrade counting. Enforce token admission limits and evaluate resolution-sensitive slices.
Visual-token cost is roughly quadratic inside full self-attention and linear in KV/decode interactions. Tiling preserves small text but duplicates overlap and loses global relations; a thumbnail supplies global context while crops supply detail. Dynamic resolution chooses tiles from aspect ratio and content under a token budget. Record crop coordinates and image IDs so grounding survives interleaving. Evaluate accuracy against tokens, TTFT, memory, and missed-small-object rate—not resolution alone.

* **Red Flag:** Maximizing tiles for every image because higher resolution must be better.
