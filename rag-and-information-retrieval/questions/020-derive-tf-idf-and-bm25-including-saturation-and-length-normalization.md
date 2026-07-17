### Q: Derive TF-IDF and BM25, including saturation and length normalization.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** TF-IDF weights terms by within-document frequency and inverse corpus frequency; BM25 adds saturating TF and adjustable document-length normalization, retaining strong exact-match behavior for entities, numbers, and rare terms.
* **The Deep Dive:** A basic TF-IDF weight is $tf(t,d)\cdot\log(N/df_t)$ with variants for log TF, smoothed IDF, and vector normalization. BM25 score commonly is

$$
\sum_{t\in q}IDF(t)\frac{tf(t,d)(k_1+1)}{tf(t,d)+k_1\left(1-b+b\frac{|d|}{avgdl}\right)},
$$

with $IDF(t)=\log\left(1+\frac{N-df_t+0.5}{df_t+0.5}\right)$ in one common implementation. As TF grows, the fraction approaches $k_1+1$: repeated terms saturate. $b=0$ disables length normalization; $b=1$ fully scales by relative length. Query TF is often omitted for short queries. Tokenization, stopwords, stemming, phrase/proximity, fields and boosts define real behavior.

For $tf=0$ contribution is zero; rare terms get high IDF. Very long chunks are penalized, linking BM25 to chunk design. Implementations differ in IDF and field scoring, so pin library/config.
* **Production Reality & Tradeoffs:** BM25 misses paraphrases but is transparent, cheap, incrementally updateable, and robust for exact identifiers. Hybrid retrieval covers semantics. Corpus changes alter IDF; version/rebuild and evaluate source/language fields.
* **Red Flag:** Saying BM25 is raw term count or setting $b=1$ because it means “maximum relevance.”

