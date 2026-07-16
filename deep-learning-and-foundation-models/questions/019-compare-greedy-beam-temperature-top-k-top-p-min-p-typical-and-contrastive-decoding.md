### Q: Compare greedy, beam, temperature, top-k, top-p, min-p, typical, and contrastive decoding.
* **Difficulty:** Mid
* **Category:** Inference
* **The 10-Second Pitch:** Greedy/beam search optimize high-probability sequences; stochastic filters reshape the next-token distribution for diversity. Temperature rescales logits, top-k fixes count, top-p fixes cumulative mass, min-p is relative to the maximum, typical favors expected-surprisal tokens, and contrastive decoding penalizes degeneration.
* **The Deep Dive:** Temperature samples $p_i\propto e^{z_i/\tau}$: $\tau<1$ sharpens, $\tau>1$ flattens, and $\tau\to0$ approaches greedy. Top-k retains the $k$ largest probabilities; nucleus/top-p retains the smallest set whose cumulative mass reaches $p$; min-p keeps tokens with probability above a fraction of the maximum. Typical sampling retains tokens whose surprisal $-\log p_i$ is near entropy, then samples. Filter ordering and renormalization are part of the algorithm.

Greedy picks local argmax and is deterministic but not globally optimal. Beam keeps multiple cumulative log-probability hypotheses; length normalization, coverage, EOS, and constraints matter, and large beams can favor generic text. Contrastive search/decoding combines probability with hidden-state degeneration penalty or compares a stronger and weaker model’s logits (two related meanings).

```text
logits -> penalties/constraints -> temperature -> probability filter -> renormalize -> sample
```
* **Production Reality & Tradeoffs:** Sampling changes reproducibility and tool/JSON validity. Aggressive filters can exclude the only correct rare token; repetition penalties distort probabilities. Choose settings by task and evaluate quality, diversity, factuality, safety, and latency. Constrained decoding is preferable for schemas.
* **Red Flag:** Calling top-p a fixed number of tokens, or using beam search as universally better for open-ended dialogue.

