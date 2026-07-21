### Q: Explain test-time compute scaling: long chain-of-thought RL, parallel sampling with verifiers, and thinking budgets versus pretraining scale.
* **Difficulty:** Principal
* **Category:** Foundation Models
* **The 10-Second Pitch:** Test-time scaling spends inference FLOPs instead of parameters: sequentially, by RL-training the model on verifiable rewards until it emits long self-correcting chains of thought, and in parallel, by sampling $n$ candidates and selecting with a verifier. Compute-matched comparisons show it beats model scaling on tasks the base model can already sometimes solve, and loses where base-model coverage is the bottleneck.
* **The Deep Dive:** Sequential scaling comes from reinforcement learning with verifiable rewards (RLVR): sample reasoning traces, score them with an exact checker (final-answer match, unit tests), and apply a policy-gradient update (PPO or GRPO) that reinforces successful traces. Response length is not imposed; it grows emergently during training because longer traces that verify, backtrack, and retry earn more reward — DeepSeek-R1's average chain length grew steadily across RL steps, and OpenAI's o1 report showed accuracy on competition math rising roughly linearly in the log of thinking tokens. This axis is inherently serial — token $t+1$ conditions on token $t$ — so it buys accuracy with wall-clock latency, but it can create qualitatively new behavior by moving probability mass toward strategies the base model rarely sampled.

  Parallel scaling draws $n$ independent samples and selects one via majority voting (self-consistency) or a verifier — an outcome/process reward model or an exact checker. Coverage, $\operatorname{pass}@n$, grows smoothly and approximately log-linearly in $n$ over many orders of magnitude ("Large Language Monkeys"), but selection can never exceed coverage. That is the limiting case: if the base model assigns essentially zero probability to any correct solution, no amount of sampling helps, whereas sequential RL can still shift the distribution. A falsifiable test separates verifier regimes: with an oracle checker, best-of-$n$ accuracy is monotone nondecreasing in $n$; with a learned reward model it typically peaks and then degrades as $n$ grows, because the argmax over more samples increasingly lands on reward-model errors. If your best-of-$n$ curve bends down, your verifier — not your sampler — is the binding constraint.

  Comparing inference scaling to pretraining scale honestly requires FLOP matching, not sample matching. A decoder forward pass costs about $2N$ FLOPs per token for $N$ parameters, so a strategy generating $n$ chains of length $L$ costs roughly

  $$
  C_{\mathrm{inf}} \approx 2\,N\,n\,L .
  $$

  Worked numbers:

  ```text
  strategy                          FLOPs/query = 2 N n L
  8B model, 1 answer, L = 500       2(8e9)(1)(500)    = 8.0e12
  8B model, long CoT, L = 8000      2(8e9)(1)(8000)   = 1.3e14
  8B model, n = 16, L = 2000        2(8e9)(16)(2000)  = 5.1e14
  70B model, 1 answer, L = 500      2(70e9)(1)(500)   = 7.0e13
  ```

  Best-of-16 with 2K-token chains on the 8B costs about 7x the 70B's single pass — quoting the small model's win without this accounting is methodologically invalid. Done properly (Snell et al., 2024), compute-optimal test-time strategies let a small model outperform a model roughly 14x larger on easy and medium difficulty bins, while on the hardest bin pretraining scale still wins: those problems sit outside the small model's coverage, so extra inference compute is spent re-sampling failure.
* **Production Reality & Tradeoffs:** The two axes have opposite operational profiles: sequential scaling turns into user-visible latency (minutes of serial, memory-bandwidth-bound decode) and long-context KV growth, while parallel scaling turns into cost and batch pressure — $n$ concurrent chains multiply KV-cache footprint and tokens billed, though latency stays near one chain. Deployed APIs expose thinking budgets or effort levels precisely because models overthink easy queries; production systems route by predicted difficulty, cap budgets per tier, and only pay for verifier infrastructure (sandboxed test execution, reward-model serving) on high-value traffic. Watch acceptance of the answer without ground truth: best-of-$n$ needs a deployable selection rule, not just an eval-time checker.
* **Red Flag:** Comparing a best-of-$n$ or long-CoT system to a bigger model at equal query counts instead of equal FLOPs, or claiming test-time compute substitutes for pretraining scale in general — parallel sampling cannot exceed base-model coverage, and a learned verifier degrades under heavy sampling exactly when you rely on it most.
