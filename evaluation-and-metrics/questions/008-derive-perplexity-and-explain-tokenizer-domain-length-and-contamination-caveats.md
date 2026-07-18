### Q: Derive perplexity and explain tokenizer, domain, length, and contamination caveats.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Perplexity is the exponentiated mean token negative log-likelihood, $\exp[-N^{-1}\sum_t\log p_\theta(x_t\mid x_{<t})]$; it is comparable only under the same tokenizer, evaluation protocol, and distribution.
* **The Deep Dive:** For tokens $x_{1:N}$, cross-entropy in nats and perplexity are

$$
H=-\frac1N\sum_{t=1}^{N}\log p_\theta(x_t\mid x_{<t}),
\qquad \operatorname{PPL}=e^H.
$$

With base-2 logs, perplexity is $2^H$. It is the geometric mean inverse probability assigned to observed next tokens. Mask padding and declare whether BOS/EOS count. Long documents require sliding windows with every target scored once and as much left context as allowed; resetting each chunk changes the conditional problem. Token-level PPL changes with tokenization, so bits per byte/character is only a rough cross-tokenizer normalization. Memorization can reduce PPL without improving instruction following or factuality.
* **Production Reality & Tradeoffs:** PPL is efficient for base-model likelihood but unavailable for many APIs and misleading for chat templates unless the exact serialization and target mask are fixed. Report token count, tokenizer, stride/context, domain, and contamination checks.
* **Red Flag:** Comparing perplexities across different tokenizers as if they measured the same random variable.
