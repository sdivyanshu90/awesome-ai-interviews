### Q: Trace tokenization, prefill, decode, sampling, detokenization, and streaming through an inference request.
* **Difficulty:** Mid
* **Category:** Inference
* **The 10-Second Pitch:** An inference server validates/tokenizes input, schedules prefill to build hidden/KV state, repeatedly decodes one or speculative tokens, samples from logits, incrementally detokenizes, and streams while enforcing stop/cancellation and resource policy.
* **The Deep Dive:** Token IDs plus masks enter prefill, which computes all prompt positions and stores per-layer K/V. Decode schedules active sequences, reads weights/cache, appends new K/V, produces last-position logits, applies processors/temperature/top-k/p, samples IDs, and checks EOS/stop/max limits. Detokenizer must handle byte fragments and stop strings across token boundaries. Scheduler releases blocks on completion/cancel and records TTFT/ITL/usage.
* **Production Reality & Tradeoffs:** Chat template/tokenizer/model versions are inseparable. Streaming can expose content before final safety validation. Cancellation and client disconnect must promptly free KV/cache.
The critical path is:
```text
HTTP/auth -> tokenize/admit -> queue -> prefill -> allocate KV
          -> repeated {schedule, decode, sample, stream} -> free KV
```
Prefill processes all prompt tokens in parallel and establishes per-layer KV; decode processes one new token per active sequence while reading prior KV. Instrument queue, tokenize, prefill, each decode iteration, detokenize, and network flush separately. Cancellation must stop scheduling and reclaim blocks even if the client disconnects mid-stream.

* **Red Flag:** Describing inference as one forward pass that returns the whole completion.
