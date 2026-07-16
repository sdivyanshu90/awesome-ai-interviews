### Q: Derive autoregressive sequence factorization and next-token negative log-likelihood.
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** The chain rule factors a sequence probability as $p(x_{1:T})=\prod_{t=1}^{T}p(x_t\mid x_{<t})$. Training minimizes the summed or token-mean negative log probability of the observed next token under a causal mask.
* **The Deep Dive:** For any discrete sequence, the probability chain rule is exact:

$$
\log p_\theta(x_{1:T})=\sum_{t=1}^{T}\log p_\theta(x_t\mid x_{<t}),\qquad
L=-\sum_{t\in\mathcal M}\log p_\theta(x_t\mid x_{<t}).
$$

$\mathcal M$ selects target tokens: padding, prompt tokens, cross-document boundaries, or ignored labels may be excluded. In implementation, input IDs $[x_1,\ldots,x_{T-1}]$ predict labels $[x_2,\ldots,x_T]$; a causal mask prevents position $t$ from using future labels. Teacher forcing supplies ground-truth prefixes during training, enabling all positions to be scored in parallel even though each conditional is causal. Per-token cross-entropy has logit gradient $p-e_y$.

Perplexity is $\exp(L/N)$ for $N$ scored tokens under a fixed tokenizer/protocol. During generation, sampled predictions become future inputs, so errors alter the visited prefix distribution—exposure mismatch. Sequence likelihood also prefers common/short continuations unless decoding/objectives compensate.
* **Production Reality & Tradeoffs:** Loss averaging by sequence versus token changes weighting. Packing must block cross-document attention and labels correctly. Data duplication, tokenizer fertility, and contamination distort comparisons. Lower NLL does not guarantee instruction following, factuality, or task success.
* **Red Flag:** Saying the model predicts every token independently, or shifting labels/masks incorrectly so a token can see itself or the next token.

