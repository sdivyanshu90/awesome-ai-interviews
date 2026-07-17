### Q: Design a real-time multimodal assistant combining video frames, audio, memory, tools, and interruption.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Use timestamped streaming ingestion, adaptive frame/audio selection, modality-specific encoders, a synchronized event state, low-latency model/tool loop, and cancellable output. Persist only confirmed, authorized memory.
* **The Deep Dive:** Treat this as an event-time system, not a giant prompt. Capture assigns monotonic timestamps; acoustic echo cancellation prevents synthesized speech from re-entering ASR, VAD/endpointer emits partial/final utterances, and an adaptive visual sampler selects keyframes or regions using scene change and motion. Encoders produce typed events in a bounded temporal buffer. A fusion service resolves clock drift, deduplicates observations, associates “that one” with tracked objects, and retrieves only permission-compatible memory. The dialogue policy consumes compact scene state plus recent evidence, streams provisional output, and emits typed tool proposals. An independent enforcement point validates principal, schema, authorization, budget, and approval.

```text
camera -> frame sampler -> vision encoder --+
                                             +-> timed event buffer -> context/policy -> model -> TTS
mic -> AEC -> VAD -> streaming ASR ----------+                          |              |
ACL-filtered memory ----------------------------------------------------+              +-> tool proposal
                                                                                              |
                                                                      auth + approval -> executor

barge-in -> increment turn generation -> cancel stale decoding, audio, and uncommitted actions
```

  Barge-in is a state transition: stop playback, cancel decoding, invalidate speculative actions using a turn/generation ID, retain committed side effects, and summarize the interrupted turn. Executors need idempotency keys and explicit commit points. Under backpressure, drop redundant frames before ordered speech/control events. Raw media and derived memory require separate retention, provenance, consent, and deletion propagation.
* **Production Reality & Tradeoffs:** Full-rate video is generally unaffordable; event-driven sampling, visual-prefix caching, and resolution escalation are essential. Clock drift, partial-ASR revision, jitter, mobile thermals, echo, and privacy complicate correctness. Budget capture, perception, first-token/audio, and interruption separately. Measure grounded task success, first-audio latency, WER under echo, barge-in stop latency, stale-action rate, authorization failures, and safety by modality—not only end-to-end latency.
* **Red Flag:** Concatenating sampled frames and transcript into a prompt with no synchronization or cancellation model.
