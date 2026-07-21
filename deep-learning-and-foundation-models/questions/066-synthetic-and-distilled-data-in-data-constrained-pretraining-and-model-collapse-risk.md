### Q: How do synthetic and distilled data change pretraining in the data-constrained regime, and when is model collapse a real risk?
* **Difficulty:** Principal
* **Category:** Data
* **The 10-Second Pitch:** Data-constrained scaling gives repetition an exponentially decaying value with a hard asymptote near sixteen effective epochs, so frontier mixtures add distilled, rephrased, and verifier-filtered synthetic tokens; collapse is real only under recursive, unfiltered self-training whose truncated sampling deletes tails — verified, provenance-capped, accumulate-not-replace mixtures avoid the mechanism.
* **The Deep Dive:** The data-constrained scaling result (Muennighoff et al., 2023) modifies Chinchilla by replacing raw dataset size with effective unique data: with $U_D$ unique tokens trained for $1 + R_D$ epochs, the fitted value is

  $$
  D' \;=\; U_D + U_D\, R_D^{*}\left(1 - e^{-R_D/R_D^{*}}\right), \qquad R_D^{*} \approx 15.4,
  $$

  so the marginal value of the $R$-th repeat decays as $e^{-R/R_D^{*}}$ and effective data saturates at $U_D(1 + R_D^{*}) \approx 16.4\,U_D$ however long you train. Worked numbers: with $U_D = 3\,\mathrm{T}$ unique tokens trained 5 epochs ($R_D = 4$), $D' = 3(1 + 15.4(1 - e^{-4/15.4})) \approx 13.6\,\mathrm{T}$ effective out of $15\,\mathrm{T}$ spent — about 91 percent efficiency — while 100 epochs of the same corpus can never exceed roughly $49\,\mathrm{T}$ effective tokens. That asymptote, against budgets that want tens of trillions of tokens, is why synthetic data enters pretraining.

  Synthetic sources differ in how much outside information they carry, and that — not the label — determines collapse risk:

  ```text
  source                          outside signal              collapse risk
  teacher distillation            stronger teacher model      low/medium (inherits teacher bias)
  rephrased human text (WRAP)     human content as anchor     low (style synthetic, content real)
  verifier-filtered generation    executor / prover / tests   low (verifier injects new bits)
  unfiltered self-generation      none                        high (recursive tail loss)
  ```

  The collapse mechanism (Shumailov et al., Nature 2024) is statistical: each generation fits the previous generation's samples, and finite sampling plus truncated decoding under-represents tails. In the tractable Gaussian version, generation $g$ fits $(\hat\mu_g, \hat\sigma_g^2)$ by maximum likelihood to $n$ draws from generation $g-1$; since the MLE variance is biased low by the factor $(n-1)/n$, expectations telescope to $\mathbb{E}[\hat\sigma_g^2] = (1 - 1/n)^g\,\sigma_0^2 \to 0$. With $n = 10^3$ samples per generation, expected variance falls to $e^{-2} \approx 0.14$ of its original value by generation 2000 — and LLM decoding is worse than this bound, because top-$p$ and low-temperature sampling assign exactly zero mass to truncated tokens, so a tail event dropped in one generation is unrecoverable thereafter. Early collapse (rare events vanish) precedes late collapse (variance implodes).

  Three things break the loop in practice. Accumulation: Gerstgrasser et al. (2024) show that training each generation on the union of original plus prior synthetic data keeps test loss bounded, while replacing the corpus collapses. Verification: a unit test, proof checker, or execution sandbox is an oracle outside the model, so rejection-sampled math and code behaves like labeled data, not self-imitation. Distillation: probability mass flows from a stronger teacher rather than recursing on self. The falsifiable test is tail telemetry across generations — held-out human-text perplexity, rare $n$-gram coverage, per-token entropy distribution: collapse predicts monotone tail shrinkage under recursive unfiltered training; anchored human data plus verification predicts stable curves. The limiting case runs the other way: as verifier precision approaches one, the synthetic label stops mattering — the verifier is the data source.
* **Production Reality & Tradeoffs:** Operationally this means provenance tags on every shard and hard caps on the unverified-synthetic fraction per domain; generation compute is significant (rejection sampling at roughly ten candidates per kept item can rival training FLOPs for math-heavy mixtures); and contamination gets nastier with a teacher in the loop, because teachers paraphrase benchmark items — n-gram decontamination misses rewrites, so embedding-similarity and judge-based matching against eval suites is required (Phi-4's report treats decontamination as a first-class pipeline stage while leaning heavily on synthetic pretraining data). Finally, the open web is itself now partially model-generated without labels, so the same tail metrics must be monitored even on nominally organic crawls.
* **Red Flag:** Invoking the data-processing inequality to claim synthetic data can never add information — the verifier, curation policy, and stronger teacher are channels outside the trained model — or dismissing collapse as myth while running the one configuration where it is demonstrated: recursive self-training with truncated sampling on replaced, not accumulated, data.
