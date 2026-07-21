### Q: Derive Multi-head Latent Attention: low-rank KV compression, decoupled RoPE, and cache size versus MHA, GQA, and MQA.
* **Difficulty:** Principal
* **Category:** Architecture
* **The 10-Second Pitch:** MLA caches one shared low-rank latent per token, $c_t = W_{DKV} h_t$ of width $d_c$, and reconstructs per-head keys and values by up-projections that are absorbed into the query and output projections at inference — so the cache shrinks from $2 H_{\mathrm{kv}} d_h$ elements per token per layer to $d_c + d_r$, where $d_r$ is a small decoupled RoPE branch required because rotation does not commute with the low-rank factorization.
* **The Deep Dive:** For hidden state $h_t \in \mathbb{R}^{d}$, MLA computes a joint KV latent $c_t^{KV} = W_{DKV} h_t \in \mathbb{R}^{d_c}$ with $d_c \ll H d_h$, then per-head keys and values $k_t^{(i)} = W_{UK}^{(i)} c_t^{KV}$ and $v_t^{(i)} = W_{UV}^{(i)} c_t^{KV}$ (queries get an analogous low-rank path $c_t^{Q} = W_{DQ} h_t$). Caching only $c_t^{KV}$ is legal because of matrix absorption. The content part of the attention logit is

  $$
  q_s^{(i)\top} k_t^{(i)} \;=\; \big(W_{UQ}^{(i)} c_s^{Q}\big)^{\top} W_{UK}^{(i)} c_t^{KV} \;=\; c_s^{Q\top}\big(W_{UQ}^{(i)\top} W_{UK}^{(i)}\big)\, c_t^{KV},
  $$

  so $W_{UK}^{(i)}$ folds into an effective query projection, and symmetrically $W_{UV}^{(i)}$ folds into $W_O$ because the head output is a linear function of $\sum_t a_t c_t^{KV}$. At decode time every head therefore attends directly over the same cached latent stream — operationally MQA with a single shared "KV head" of width $d_c$ — while training-time expressivity matches distinct per-head $k, v$ subspaces.

  RoPE breaks this absorption, which is why MLA needs a decoupled branch. RoPE multiplies keys by a position-dependent block-rotation $R_t$, so the logit becomes $\big(W_{UQ} c_s^{Q}\big)^{\top} R_s^{\top} R_t\, W_{UK} c_t^{KV} = \big(W_{UQ} c_s^{Q}\big)^{\top} R_{t-s} W_{UK} c_t^{KV}$: the relative rotation $R_{t-s}$ sits between the two up-projections and depends on the query-key offset, so no single static matrix can absorb $W_{UK}$ — you would have to cache fully rotated per-head keys, destroying the compression. The fix is to carry position in a separate narrow channel: a shared rotary key $k_t^{R} = R_t W_{KR} h_t \in \mathbb{R}^{d_r}$ (one per token, shared by all heads) and per-head rotary queries, with the logit being the sum of the content term and the rotary term, scaled by $\sqrt{d_h + d_r}$. The cache per token per layer is then $d_c + d_r$ elements, versus $2 H d_h$ for MHA, $2 H_{\mathrm{kv}} d_h$ for GQA, and $2 d_h$ for MQA.

  Worked numbers with DeepSeek-V2/V3-style dimensions — $H = 128$ heads, $d_h = 128$, $d_c = 512 = 4 d_h$, $d_r = 64$, 61 layers, BF16:

  ```text
  per token per layer (elements)
  MHA      2 * 128 * 128 = 32768
  GQA-8    2 *   8 * 128 =  2048
  MQA      2 *   1 * 128 =   256
  MLA      512 + 64      =   576   (57x vs MHA, 3.6x vs GQA-8)

  full model, BF16: 576 * 61 * 2 B = 68.6 KB/token
  at 128K context: about 9.2 GB/sequence  (MHA: about 3.8 MB/token, 524 GB)
  ```

  The falsifiable claim is that this is compression, not truncation: DeepSeek-V2's ablations show MLA matching or beating full MHA quality at this cache size — possible only because trained MHA KV heads are highly redundant. The limiting case: as $d_c \to 2 H d_h$ the latent becomes full-rank and MLA degenerates into MHA with extra matmuls and zero cache savings; all of the benefit lives in choosing $d_c$ far below the ambient KV width without losing the task-relevant subspace.
* **Production Reality & Tradeoffs:** MLA trades cache bytes for compute and kernel complexity: absorbed-form decode does GEMMs against a $d_c + d_r$ stream (great for memory-bandwidth-bound decoding, larger batches, longer contexts), but prefill often runs the unabsorbed form, and naive implementations that materialize per-head $k, v$ forfeit the entire win. Serving stacks needed new kernels and paged-cache layouts (one latent page replaces per-head KV pages), and quality should be re-verified after any long-context extension since the rotary branch is a narrow $d_r$-dim positional bottleneck.
* **Red Flag:** Claiming the KV up-projections are applied per cached token at decode (they are absorbed into $W_Q$ and $W_O$), or applying RoPE to the compressed keys directly — the rotation sits between the factors, does not commute with the low-rank factorization, and silently forces caching full rotated keys.
