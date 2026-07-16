### Q: Implement BPE training and encoding step by step.
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** Initialize text as base symbols, repeatedly merge the most frequent adjacent pair into a new token, and encode new text by applying learned merges in rank order. Production BPE additionally fixes normalization, byte fallback, pre-tokenization, special tokens, and deterministic tie-breaking.
* **The Deep Dive:** Create an initial alphabet—often bytes for total coverage—and represent each training word/span as a symbol sequence with frequency. Count adjacent pairs weighted by corpus frequency, choose the highest-frequency pair, append it to vocabulary with the next merge rank, replace all nonoverlapping occurrences, update affected counts, and repeat until the vocabulary budget is met. Encoding starts from base symbols and repeatedly applies the available lowest-rank merge; efficient implementations use a priority queue/linked neighbors rather than rescanning.

```text
low lower lowest
l o w      -> merge (l,o) => lo w
lo w e r   -> merge (lo,w) => low e r
low e s t  -> learned ranks determine final segmentation
```

Define Unicode normalization, whitespace/pre-tokenization regex, byte-to-visible mapping, unknown/byte fallback, BOS/EOS/control tokens, and whether merges cross word boundaries. Decoding concatenates token byte strings then decodes UTF-8; special tokens need explicit handling. Embedding lookup maps IDs $[B,T]$ through table $E\in\mathbb R^{V\times D}$ to $[B,T,D]$.
* **Production Reality & Tradeoffs:** Naive training is expensive; sharded counts and deterministic merges matter. Tokenizer changes invalidate embeddings, checkpoints, caches, context/cost estimates, and evaluation comparability. Test round-trip bytes, adversarial Unicode, spaces, code, numbers, and special-token injection.
* **Red Flag:** Describing BPE as splitting on spaces, or applying merges in frequency order recomputed at inference rather than learned merge rank.

