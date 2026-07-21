### Q: Compare encoder-only, encoder-decoder, decoder-only, prefix, and unified architectures.
* **Difficulty:** Mid
* **Category:** Architecture
* **The 10-Second Pitch:** Architecture is largely an attention-mask and interface choice: encoders use bidirectional context, decoders causal context, encoder-decoder models add cross-attention to a separate source, and prefix/unified models combine visibility patterns in one stack.
* **The Deep Dive:** Encoder-only models let every nonpadding token attend bidirectionally and learn contextual representations, suiting classification, tagging, retrieval, and masked objectives. Decoder-only models apply a causal mask and factor output autoregressively; prompts and outputs share one stream, simplifying scaling/tool/chat interfaces. Encoder-decoder models encode a source bidirectionally, then a causal decoder cross-attends to encoder memory; source representations are computed once and not expanded in decoder KV like repeated prefix self-attention.

Prefix LMs allow a prefix region to attend bidirectionally while generated suffix attends to prefix and earlier suffix. Unified architectures use token/segment types and programmable masks to emulate encoding, infilling, and generation in one parameter stack.

```text
encoder:         source <------> source
encoder-decoder: source <------> source  => causal target via cross-attention
decoder-only:    prompt + target, every position sees only allowed left context
prefix LM:       prefix <------> prefix  => causal suffix
```

A falsifiable test: train decoder-only, prefix-LM, and encoder-decoder variants at matched parameter count and training tokens on one corpus, then evaluate source-conditioned generation (summarization/translation) at matched decode latency and batch size. If encoder-decoder shows no advantage on source-heavy tasks under this control, the claimed benefit was scale or data, not the mask/interface choice.
* **Production Reality & Tradeoffs:** Decoder-only systems benefit from common infrastructure/data but spend causal compute modeling source tokens. Encoder-decoder can be more efficient for conditional generation and explicit source separation. Mask bugs and chat templates can erase theoretical advantages; compare at matched parameters/tokens/latency.
* **Red Flag:** Saying encoder-decoder means two independent models, or that decoder-only attention is bidirectional over the prompt unless the mask explicitly permits it.

