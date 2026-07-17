### Q: Design streaming ASR with VAD, endpointing, chunking, timestamps, punctuation, and partial hypotheses.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Stream audio through VAD and chunked encoder state, emit stable partial hypotheses, determine endpoint from acoustic/linguistic signals, then finalize timestamps, punctuation, and diarization with bounded revision.
* **The Deep Dive:** VAD gates silence but must preserve breaths/quiet speech. Chunks overlap or carry recurrent/attention cache; right context trades latency for accuracy. Decoder maintains beams/RNN-T state. Partial tokens have stability scores and may be revised; UI distinguishes unstable text. Endpoint combines silence duration, decoder completion probability, and maximum utterance. Word timestamps derive alignment or token timing; punctuation/casing may run incrementally/finally.
* **Production Reality & Tradeoffs:** Noise, cross-talk, packet loss, and barge-in drive tails. False endpoints split meaning; late endpoints hurt responsiveness. Measure WER, partial churn, endpoint latency, real-time factor, and memory.
Streaming ASR maintains encoder/cache state across overlapping chunks, uses VAD to suppress silence, and commits words only after alignment stability or a latency deadline. Partial hypotheses need revision IDs so clients replace rather than append text.
```text
audio -> echo/VAD -> feature chunks -> streaming encoder -> decoder
      -> unstable suffix / committed prefix -> punctuation -> timestamps
```
Endpointing balances premature cutoff against delay. Test speech after pauses, code-switching, background media, packet loss, and overlapped speakers.

* **Red Flag:** Optimizing final WER while ignoring partial stability and endpoint latency.
