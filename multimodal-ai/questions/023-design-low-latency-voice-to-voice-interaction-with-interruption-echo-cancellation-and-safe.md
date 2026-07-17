### Q: Design low-latency voice-to-voice interaction with interruption, echo cancellation, and safety.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Pipeline streaming audio through AEC/VAD/ASR, incremental dialogue/tool planning, and streaming TTS; support barge-in by cancelling generation/playback and preserving only confirmed state. Safety decisions must finish before risky speech/actions.
* **The Deep Dive:** Client/server timestamps align audio. Acoustic echo cancellation removes assistant playback from microphone input; VAD detects user onset. Stable ASR prefixes can trigger speculative response planning, but actions wait for sufficient certainty. TTS emits cancellable chunks; interruption invalidates pending speech and tool calls unless committed. Conversation state records confirmed versus provisional transcript. Content/tool policies run incrementally with safe fallback.
* **Production Reality & Tradeoffs:** Every stage contributes tail latency and errors cascade. Network jitter needs buffers; buffers add delay. Test noisy rooms, duplex speech, accents, partial words, and cancellation races. Never let speculative text execute side effects.
The latency budget spans endpoint detection, ASR first partial, semantic processing, tool calls, TTS first audio, and network jitter. Full-duplex operation needs acoustic echo cancellation so synthesized speech is not re-transcribed, plus barge-in detection that cancels generation and side effects safely.
```text
mic -> AEC/VAD -> streaming ASR -> turn manager -> model/tools -> streaming TTS
 ^                         | barge-in/cancel                 |
 +-------------------------+---------------------------------+
```
Commit transactional actions separately from conversational interruption.

* **Red Flag:** Treating voice as text chat with ASR and TTS wrappers.
