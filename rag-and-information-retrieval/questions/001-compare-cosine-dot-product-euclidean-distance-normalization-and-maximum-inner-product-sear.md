### Q: Compare cosine, dot product, Euclidean distance, normalization, and maximum inner-product search.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** Cosine ranks directions, dot product ranks direction plus norm, and Euclidean ranks distance. For unit-normalized vectors, maximizing cosine/dot and minimizing squared Euclidean are identical; otherwise the embedding training objective determines which geometry is valid.
* **The Deep Dive:** Cosine is $q^Td/(\|q\|\|d\|)$. If $\|q\|=\|d\|=1$,

$$
\|q-d\|_2^2=\|q\|^2+\|d\|^2-2q^Td=2-2q^Td,
$$

so rankings coincide. Without document normalization, maximum inner product search (MIPS) lets norm encode confidence, popularity, length, or training artifacts; converting to cosine discards that information. Euclidean distance is translation/scale-sensitive and some ANN indexes require a declared metric. Zero vectors make cosine undefined.

Normalize consistently at both indexing and query time, using the embedding model’s documented query/document prefixes and objective. Score magnitudes are not relevance probabilities and cannot be compared across models or queries without calibration. MIPS can be reduced to Euclidean search by asymmetric dimension transforms under bounded norms, but native support is preferable.

```text
unit vectors: highest dot = highest cosine = lowest L2
raw vectors:  norm changes dot and L2 rankings
```
* **Production Reality & Tradeoffs:** Normalization costs little but changes model semantics. Quantization perturbs norms/angles. ANN recall depends on metric-compatible construction; changing metric requires rebuilding. Evaluate task relevance and slices, not geometric intuition alone.
* **Red Flag:** Saying cosine is always better because it removes magnitude, or mixing normalized query with unnormalized corpus accidentally.

