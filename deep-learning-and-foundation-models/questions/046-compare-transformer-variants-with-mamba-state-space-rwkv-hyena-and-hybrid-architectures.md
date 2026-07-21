### Q: Compare Transformer variants with Mamba/state-space, RWKV, Hyena, and hybrid architectures.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Transformers retain explicit token-addressable KV; SSM/Mamba, RWKV, and Hyena use recurrent or long-convolutional compressed state with near-linear sequence scaling. Hybrids reserve attention for exact content lookup and efficient layers for most mixing.
* **The Deep Dive:** Softmax attention provides data-dependent pairwise retrieval and parallel training but quadratic prefill/growing decode cache. Structured state-space models evolve a latent linear state; selective SSMs make parameters/input gates data-dependent and use parallel scans for training plus recurrent state for streaming. RWKV mixes time-decay/receptance recurrence in a Transformer-like channel stack, training with parallel formulations and decoding fixed state. Hyena-style operators use implicit long convolutions plus gating, avoiding explicit attention matrices. Exact formulas vary by generation; compare operator, state, and kernels rather than brand names.

Compressed-state models have $O(1)$ state per layer at decode and near-linear long training, but must encode history into finite dimensions and may struggle with arbitrary exact recall, copying, or in-context binding. Attention has growing memory but directly revisits stored tokens. Hybrids interleave layers or add local/global attention. Mamba-2's state-space duality (SSD) reframes selective SSMs as structured masked attention, yielding tensor-core-friendly kernels; named production hybrids include Jamba (attention plus Mamba plus MoE), Griffin-class gated-linear-recurrence/local-attention stacks, and hybrid Nemotron models that swap most attention layers for Mamba-2 blocks.

```text
attention: history stored explicitly as K/V -> query retrieves
SSM/RWKV: history folded into recurrent state -> state updates
Hyena: history mixed through long convolution/gates
```
* **Production Reality & Tradeoffs:** Hardware kernels and length determine real crossover. Evaluate language loss plus recall, induction, copying, long distractors, streaming resets, state quantization, and latency. Ecosystem/maturity matter.
* **Red Flag:** Claiming linear-time architecture is universally faster/better, or that fixed state stores every past token losslessly.

