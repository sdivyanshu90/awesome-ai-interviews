### Q: Evaluate ASR/TTS using WER/CER, speaker similarity, intelligibility, naturalness, latency, and fairness.
* **Difficulty:** Principal
* **Category:** Evaluation
* **The 10-Second Pitch:** Use error metrics and human/task measures by acoustic and demographic slice. WER/CER measure transcription edits; TTS requires intelligibility, naturalness, speaker/prosody fidelity, safety, and streaming performance.
* **The Deep Dive:** WER `(S+D+I)/N` is sensitive to normalization and can exceed 100%; CER helps languages without word boundaries. ASR evaluation includes timestamp/diarization and entity errors. TTS intelligibility can use ASR proxy plus listener tests; MOS has scale/rater bias. Speaker embedding similarity can reward identity but miss naturalness/spoofing. Measure TTFA, real-time factor, partial churn, barge-in response, and failures by language, accent, gender, age, disability, noise, and device.
* **Production Reality & Tradeoffs:** Automated metrics correlate imperfectly with users. Consent and privacy constrain voice datasets. Report confidence intervals and rater protocol.
WER is $(S+D+I)/N$ after a declared normalization; CER is better for languages without whitespace and OCR-like errors. Neither measures semantic harm: substituting a dosage and an article both count once. TTS needs intelligibility, pronunciation, speaker similarity, naturalness/MOS, prosody, safety, and real-time latency. Report bootstrap confidence intervals and slices by accent, dialect, noise, speaker, language, and utterance length. Speaker similarity can reward identity cloning, so pair it with consent and spoofing evaluations.

* **Red Flag:** Publishing one aggregate WER or MOS as universal quality.
