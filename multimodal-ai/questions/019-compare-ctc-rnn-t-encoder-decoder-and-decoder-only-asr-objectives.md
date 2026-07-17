### Q: Compare CTC, RNN-T, encoder-decoder, and decoder-only ASR objectives.
* **Difficulty:** Senior
* **Category:** Speech
* **The 10-Second Pitch:** CTC marginalizes monotonic alignments with blanks; RNN-T adds a prediction network for streaming label history; encoder-decoder attention models flexible sequence alignment; decoder-only models serialize audio tokens/features and text under causal prediction.
* **The Deep Dive:** CTC assumes conditional independence of output labels given encoder frames and sums valid blank/repeat paths via dynamic programming. RNN-T joint network combines acoustic time and output-prefix states, also marginalizing monotonic paths. Attention encoder-decoder predicts tokens from full/streamed encoded audio with learned attention. Decoder-only multimodal models condition causal text on projected audio features, enabling instruction following but often at higher latency.
* **Production Reality & Tradeoffs:** CTC is simple/fast but relies on external/internal LM for rich context. RNN-T supports low-latency streaming but is complex. Attention models need chunking for stream. Evaluate WER plus latency and timestamps.
CTC sums over monotonic alignments with blanks and assumes conditional independence across output steps; RNN-T adds a prediction network for streaming label history; attention encoder-decoder models flexible alignments but is less naturally streaming; decoder-only systems serialize audio tokens/features and text into one causal objective. The choice changes beam search, timestamps, endpointing, and compute. Evaluate with identical language-model assistance and streaming constraints; offline WER comparisons otherwise obscure deployment behavior.

* **Red Flag:** Saying CTC directly learns one frame-to-one-character alignment.
