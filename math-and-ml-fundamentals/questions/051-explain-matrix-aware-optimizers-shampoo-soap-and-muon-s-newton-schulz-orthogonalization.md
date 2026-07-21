### Q: Explain matrix-aware optimizers: Shampoo, SOAP, and Muon’s Newton–Schulz orthogonalization.
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** Adam-family optimizers precondition every scalar independently and ignore that a weight matrix’s gradient has correlated rows and columns. Shampoo and SOAP apply Kronecker-factored second-order preconditioning; Muon orthogonalizes the momentum matrix with a Newton–Schulz iteration, which is steepest descent under the spectral norm. Hidden matrices get the matrix-aware update; embeddings, norm gains, and 1D parameters stay on AdamW.
* **The Deep Dive:** The gradient $G_t\in\mathbb{R}^{m\times n}$ of a hidden linear layer typically has a fast-decaying singular spectrum, so a per-coordinate Adam step is dominated by a few singular directions and barely moves the rest. Shampoo fixes this at the matrix level: it accumulates $L_t=L_{t-1}+G_tG_t^\top$ and $R_t=R_{t-1}+G_t^\top G_t$ and updates with $\Delta W_t\propto -L_t^{-1/4}G_tR_t^{-1/4}$, a Kronecker-factored approximation of full-matrix AdaGrad whose exact preconditioner would be $mn\times mn$. SOAP keeps Shampoo’s slowly refreshed preconditioner eigenbases but runs Adam inside that rotated basis, which removes most of the sensitivity to how often you recompute the eigendecompositions. Muon is the cheapest member: keep only a momentum buffer $M_t=\mu M_{t-1}+G_t$ (Nesterov-style, $\mu\approx 0.95$) and replace it by its nearest semi-orthogonal matrix,

  $$
  \mathrm{Orth}(M_t)=UV^\top\ \text{ for }\ M_t=U\Sigma V^\top,
  \qquad
  W_{t+1}=W_t-\eta_t\,0.2\sqrt{\max(m,n)}\,\mathrm{Orth}(M_t),
  $$

  where the Moonlight-style scale sets the update RMS to about $0.2$ so AdamW learning rates and weight decay transfer. Since every singular value of $\mathrm{Orth}(M_t)$ equals 1, the update’s condition number is 1: all directions in the momentum’s row/column space move equally, which is exactly steepest descent under a spectral-norm constraint on the step.

  Muon never computes the SVD. It normalizes $X_0=M_t/\lVert M_t\rVert_F$ (pushing all singular values into $(0,1]$) and runs five iterations of the quintic map $X\leftarrow aX+b\,(XX^\top)X+c\,(XX^\top)^2X$ with $(a,b,c)=(3.4445,\,-4.7750,\,2.0315)$, which acts on each singular value as the odd polynomial $f(\sigma)=a\sigma+b\sigma^3+c\sigma^5$; the slope $f'(0)=3.4445$ inflates tiny singular values quickly, and the iteration is tuned to land near 1 rather than converge exactly. Worked example: $M=\mathrm{diag}(3,\,0.1)$ has condition number 30. After normalization $\sigma=(0.999,\,0.033)$; iterating $f$ maps $0.033\to 0.115\to 0.388\to 1.075\to 0.688\to 1.129$ while the large value oscillates $0.999\to 0.701\to 1.114\to 0.721\to 1.090\to 0.697$. Both land in roughly $[0.7,\,1.2]$, so the update’s condition number falls from 30 to about 1.6 using ten small matmuls that run fine in bf16.

  ```text
  per hidden matrix W (m x n), each step:
    G  -> M = mu*M + G                       momentum: the only Muon state (Adam keeps 2 buffers)
    X  = M / frobenius_norm(M)               singular values now in (0, 1]
    5x: X = 3.4445*X - 4.7750*(X X^T) X + 2.0315*(X X^T)^2 X
    O ~= U V^T                               spectrum flattened to ~[0.7, 1.2]
    W  = W - lr * 0.2*sqrt(max(m, n)) * O    RMS-matched to AdamW-scale updates
  embeddings, unembedding, norms, biases     -> separate plain AdamW group
  ```

  Evidence and limits. Distributed Shampoo won the external-tuning track of the 2024 MLCommons AlgoPerf benchmark over a tuned AdamW baseline; SOAP reported further step-count reductions over both AdamW and Shampoo in few-hundred-million-parameter LM pretraining; Muon set nanoGPT speedrun records (roughly 1.35x faster than tuned AdamW to the fixed validation-loss target at introduction), and Moonshot validated it at scale — Moonlight (16B MoE) reported about 2x compute efficiency versus AdamW, and Kimi K2 pretrained a 1T-parameter MoE on 15.5T tokens with MuonClip, adding QK-clip because attention logits grew unboundedly under raw Muon. The limiting case tells you when it cannot help: if $M_t$ is already isotropic ($\Sigma\propto I$), then $\mathrm{Orth}(M_t)\propto M_t$ and Muon degenerates into normalized momentum SGD — gains come only from anisotropic gradients. Orthogonalization is also meaningless for vectors and lookup-table-like parameters, which is why embeddings, the unembedding, RMSNorm gains, and biases stay on AdamW in every serious deployment.
* **Production Reality & Tradeoffs:** Muon halves optimizer state versus AdamW (momentum only) and Newton–Schulz adds only a few percent FLOPs, but under FSDP/ZeRO the full momentum matrix must be gathered per layer, so a distributed implementation (as in Moonlight) is communication-engineering work; Shampoo pays $m^2+n^2$ preconditioner memory per matrix plus periodic eigendecompositions, and its win evaporates if the refresh cadence is mistuned. Track update RMS per parameter group and the max attention logit — logit blowup is the known large-scale failure that QK-clip exists to stop. Short fine-tunes and small well-tuned-AdamW models often show no wall-clock benefit.
* **Red Flag:** Applying Muon to every tensor — embeddings, norm gains, biases — or claiming Newton–Schulz computes an exact SVD; the design is approximate orthogonalization of hidden-layer matrices only, with the 1D and embedding parameters deliberately left on AdamW.
