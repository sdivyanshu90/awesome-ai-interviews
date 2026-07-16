### Q: Compare BPE, WordPiece, Unigram LM, SentencePiece, byte-level, and character tokenization.
* **Difficulty:** Senior
* **Category:** Tokenization
* **The 10-Second Pitch:** BPE greedily learns merges, WordPiece greedily encodes with likelihood-inspired vocabulary, Unigram prunes a probabilistic token inventory, SentencePiece is a raw-text framework, and byte/character units guarantee coverage with longer sequences.
* **The Deep Dive:** BPE starts from base symbols and repeatedly merges frequent adjacent pairs; encoding applies learned merge ranks. WordPiece training implementations vary, but encoding commonly uses longest-match-first over a vocabulary with continuation markers. Unigram LM starts with a large candidate vocabulary, fits token probabilities, and prunes tokens with least likelihood contribution; Viterbi finds the best segmentation and sampling can regularize. SentencePiece is not one algorithm: it provides language-independent raw-text BPE/Unigram with whitespace represented explicitly. Byte-level tokenization maps UTF-8 bytes to a fixed alphabet, eliminating unknowns but splitting non-ASCII characters into multiple bytes; character tokenization uses Unicode codepoints/graphemes with vocabulary/normalization issues.

| Choice | Coverage | Sequence length | Segmentation |
|---|---:|---:|---|
| subword | fallback-dependent | medium | learned |
| byte | complete bytes | long multilingual | deterministic base + merges |
| character | Unicode-dependent | long | codepoint/grapheme |

Vocabulary size trades embedding/output cost against context fertility. Special tokens and normalization are part of the model ABI.
* **Production Reality & Tradeoffs:** Compare bytes/tokens per language, code/math/numbers, compression, round trip, and downstream quality. Tokenizer changes invalidate weights/caches and can create security issues via invisible Unicode or special-token injection. No algorithm is universally language-neutral.
* **Red Flag:** Calling SentencePiece a distinct merge algorithm, or judging multilingual fairness only by vocabulary entries rather than fertility and quality.

