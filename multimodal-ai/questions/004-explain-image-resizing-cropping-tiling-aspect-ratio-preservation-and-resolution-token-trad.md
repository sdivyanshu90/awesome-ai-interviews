### Q: Explain image resizing, cropping, tiling, aspect-ratio preservation, and resolution/token trade-offs.
* **Difficulty:** Senior
* **Category:** Multimodal
* **The 10-Second Pitch:** Preprocessing determines which visual evidence reaches the model. Letterboxing preserves geometry with padding; cropping can discard evidence; warping distorts shapes; tiling preserves detail but multiplies tokens and requires global context.
* **The Deep Dive:** A fixed square encoder often resizes shortest/longest side then crops or pads. Any-resolution systems choose a tile grid, encode tiles plus a thumbnail, and serialize their positions. Overlap protects boundary objects but duplicates content. Token budget is approximately tiles times patches per tile; dense attention then grows quadratically unless resampled. Coordinate transforms must map predictions/citations back to source pixels.
* **Production Reality & Tradeoffs:** High resolution helps OCR and small objects but increases prefill, KV, and latency. Tiling can cause duplicate counting and cross-tile relation failures. Match training and serving preprocessing and test camera orientation, panoramas, tiny text, and extreme aspect ratios.
* **Red Flag:** Increasing resolution without accounting for token count or spatial stitching.

