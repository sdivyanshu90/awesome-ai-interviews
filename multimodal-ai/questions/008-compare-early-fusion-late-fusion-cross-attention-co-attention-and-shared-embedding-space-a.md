### Q: Compare early fusion, late fusion, cross-attention, co-attention, and shared embedding-space architectures.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Early fusion mixes modality tokens in one backbone; late fusion combines independently encoded outputs; cross-attention lets one modality query another; co-attention updates both; shared spaces align global representations. Fusion depth trades interaction quality against compute and modularity.
* **The Deep Dive:** Early fusion enables unrestricted interaction but requires aligned tokenization and expensive joint attention. Late fusion is cacheable and robust to missing modalities but captures limited fine-grained relations. Cross-attention uses Q from one stream and K/V from another; gated layers can preserve pretrained behavior initially. Co-attention alternates directions. Shared embeddings are strong for retrieval but weak for compositional grounding without token-level interaction.
* **Production Reality & Tradeoffs:** Choose based on task granularity, modality availability, latency, and ability to precompute encoders. Fusion can overfit one dominant modality; evaluate modality dropout and conflict cases.
Early fusion concatenates tokens before a shared backbone and enables rich interactions at quadratic cost. Late fusion combines independent decisions cheaply but cannot repair missing cross-modal features. Cross-attention uses one modality as queries over another’s keys/values; co-attention updates both directions; shared-embedding systems optimize global alignment for retrieval. Choose based on required interaction granularity.
```text
early: [vision tokens | text tokens] -> shared blocks
cross: text queries -> attend(vision K,V)
late:  f(image), g(text) -> score/fuse
```

* **Red Flag:** Calling concatenation automatically equivalent to multimodal fusion.
