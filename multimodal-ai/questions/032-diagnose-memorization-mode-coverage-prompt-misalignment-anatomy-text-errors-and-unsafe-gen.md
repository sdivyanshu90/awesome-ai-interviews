### Q: Diagnose memorization, mode coverage, prompt misalignment, anatomy/text errors, and unsafe generation.
* **Difficulty:** Principal
* **Category:** Debugging
* **The 10-Second Pitch:** Separate data memorization, sampling collapse, conditioning failure, representation limits, and safety-policy failure using nearest-neighbor search, controlled seeds/prompts, component ablations, and human review.
* **The Deep Dive:** Memorization tests compare generated samples/features to training candidates and use canaries/near-duplicate detection. Mode coverage uses precision/recall and category distribution, not FID alone. Prompt misalignment can originate in tokenizer/text encoder, cross-attention, CFG, or biased data. Anatomy/spelling expose weak structural/discrete modeling and VAE resolution. Unsafe output may bypass text-only filters through benign prompts or image-conditioned edits.
* **Production Reality & Tradeoffs:** Filters can overblock while failing novel content. Train-data access may be restricted, complicating memorization audits. Maintain hidden red-team and provenance pipelines.
Memorization is tested through nearest-neighbor and extraction analysis against the licensed training corpus, not visual intuition. Mode coverage requires class/attribute combinations and diversity metrics; prompt misalignment needs compositional counterfactuals; anatomy and rendered text need specialized detectors/OCR plus humans. Separate VAE reconstruction errors from denoiser/conditioning errors by encoding and decoding ground truth. Safety evaluation includes content, identity misuse, provenance, and adversarial image/text conditioning.

* **Red Flag:** Calling every repeated style memorization or relying on one image similarity threshold.
