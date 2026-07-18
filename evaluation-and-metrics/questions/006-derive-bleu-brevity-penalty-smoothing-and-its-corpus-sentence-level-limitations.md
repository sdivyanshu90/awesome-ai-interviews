### Q: Derive BLEU, brevity penalty, smoothing, and its corpus/sentence-level limitations.
* **Difficulty:** Senior
* **Category:** NLP Metrics
* **The 10-Second Pitch:** BLEU is the brevity-penalized geometric mean of clipped n-gram precisions against references; it was designed for corpus-level translation comparison, not semantic correctness of individual answers.
* **The Deep Dive:** For each n-gram order $n$, clip candidate counts by maximum reference counts to obtain modified precision $p_n$. Then

$$
\operatorname{BLEU}=BP\exp\!\left(\sum_{n=1}^{N}w_n\log p_n\right),\qquad
BP=\begin{cases}1,&c>r,\\ \exp(1-r/c),&c\le r.\end{cases}
$$

Here $c$ is candidate length and $r$ is effective reference length; the penalty prevents a one-word candidate from winning on precision. Corpus BLEU aggregates counts before the geometric mean. At sentence level, one missing higher-order n-gram makes the unsmoothed product zero, so smoothing adds pseudocounts or backs off. Tokenization, casing, punctuation, reference choice, smoothing, and effective-length rules are part of the metric.
* **Production Reality & Tradeoffs:** BLEU rewards lexical overlap, misses paraphrases and factual errors, and is unstable with few references or short samples. Never compare scores produced by different tokenizers/signatures. Pair it with learned/semantic metrics and blinded human adequacy checks.
* **Red Flag:** Calling BLEU an accuracy percentage or using sentence BLEU without naming smoothing and tokenization.
