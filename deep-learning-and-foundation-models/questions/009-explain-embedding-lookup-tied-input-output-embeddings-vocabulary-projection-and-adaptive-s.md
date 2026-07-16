### Q: Explain embedding lookup, tied input/output embeddings, vocabulary projection, and adaptive softmax.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Input embedding selects rows of $E\in\mathbb R^{V\times D}$; output projection computes logits over $V$. Weight tying uses $E^T$ as the output matrix, reducing parameters and aligning spaces. Large vocabularies make projection/softmax expensive, motivating sharding or adaptive methods.
* **The Deep Dive:** For token IDs $I\in\mathbb N^{B\times T}$, lookup returns $X_{bt:}=E_{I_{bt}:}$; its gradient is a scatter-add into rows used, so repeated IDs accumulate. After final hidden state $h\in\mathbb R^D$, logits are $z=W_{out}h+b$, $W_{out}\in\mathbb R^{V\times D}$. Training all positions costs roughly $O(BTDV)$ and stores/streams large logits; inference usually projects only the newest token.

Weight tying sets $W_{out}=E$ (orientation by convention), cutting roughly $VD$ parameters and imposing a shared token geometry, while input/output roles and optional bias remain different. Vocabulary/tensor parallelism shards rows and requires distributed reductions/top-k. Adaptive softmax clusters rare tokens and computes a shortlist plus selected tail cluster during training; sampled/hierarchical softmax approximate normalization. These methods complicate exact likelihood and decoding, so modern dense LLMs often use optimized full projections.
* **Production Reality & Tradeoffs:** Tokenizer vocabulary trades sequence length against $V$ projection/embedding cost and multilingual fertility. Tying requires compatible dimensions and may constrain specialization. Quantization and kernel fusion help, but distributed top-k/sampling correctness must be maintained.
* **Red Flag:** Claiming embedding lookup is a one-hot matrix multiply in production, or that tying means input and output probabilities are identical.

