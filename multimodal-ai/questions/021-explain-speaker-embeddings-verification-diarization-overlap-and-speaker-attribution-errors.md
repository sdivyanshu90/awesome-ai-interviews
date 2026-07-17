### Q: Explain speaker embeddings, verification, diarization, overlap, and speaker-attribution errors.
* **Difficulty:** Senior
* **Category:** Audio
* **The 10-Second Pitch:** Speaker embeddings represent voice identity; verification compares claimed speaker; diarization segments and clusters who spoke when. Overlapping speech and domain variation break the simple one-speaker-per-frame assumption.
* **The Deep Dive:** Train embeddings with classification/metric objectives, normalize, and score cosine/PLDA. Verification selects threshold from false-accept/false-reject costs. Diarization chains speech activity, segmentation, embeddings, clustering, and optional resegmentation; speaker count may be unknown. Overlap detection enables multi-label segments. Attribution maps transcript words to speaker segments, so ASR timing errors propagate.
* **Production Reality & Tradeoffs:** Microphones, illness, emotion, language, spoofing, and short segments shift embeddings. Voice is biometric data requiring consent/security. Report DER plus attribution-aware WER and overlap slices.
Speaker verification compares an enrollment embedding with an utterance under a calibrated threshold; diarization clusters/segments “who spoke when” without necessarily knowing identity. A pipeline combines VAD, embeddings, change detection/clustering, and overlap handling. DER decomposes missed speech, false alarm, and speaker confusion but can hide attribution damage on short critical turns. Thresholds shift across microphones/languages; never treat a similarity score as authenticated identity without anti-spoofing and policy controls.

* **Red Flag:** Treating cluster IDs as persistent real-world identities.
