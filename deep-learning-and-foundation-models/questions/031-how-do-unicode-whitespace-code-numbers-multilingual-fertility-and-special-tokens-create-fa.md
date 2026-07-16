### Q: How do Unicode, whitespace, code, numbers, multilingual fertility, and special tokens create failure modes?
* **Difficulty:** Senior
* **Category:** Tokenization
* **The 10-Second Pitch:** Tokenization is a security, cost, and capability boundary: normalization can merge or split visually similar text, whitespace carries code structure, number fragmentation harms arithmetic, high fertility penalizes languages, and special tokens can alter control semantics.
* **The Deep Dive:** Unicode has codepoints, combining marks, normalization forms, bidirectional controls, confusables, and grapheme clusters. NFC/NFKC choices can improve canonicalization but NFKC may erase meaningful distinctions. UTF-8 byte fallback guarantees coverage yet increases tokens for many scripts. Whitespace/newline/indentation is semantic in code and tables; normalization or pre-tokenization can destroy it. Numbers split inconsistently by length/separators/sign/exponent, weakening digit-level algorithms and making small formatting changes alter many IDs. Fertility—tokens per character/word/byte—changes context capacity, latency, cost, and learning signal across languages.

Special/control tokens must never be recognized from untrusted raw text unless escaped; tokenizer/parser disagreement can inject roles or terminate segments. Added tokens change longest-match behavior. Code operators, paths, camelCase, and indentation need dedicated tests.

```text
raw Unicode -> normalization -> pre-tokenization -> subword/byte encoding -> special-token policy
(each arrow can change semantics, security, length, and reversibility)
```
* **Production Reality & Tradeoffs:** More normalization improves compression but may harm names, math, source code, and forensic fidelity. Larger vocabularies reduce fertility but increase embedding/output parameters. Log raw bytes and token IDs carefully because prompts can contain secrets.
* **Red Flag:** Testing only English sentences or assuming visually identical strings have identical tokenization.

