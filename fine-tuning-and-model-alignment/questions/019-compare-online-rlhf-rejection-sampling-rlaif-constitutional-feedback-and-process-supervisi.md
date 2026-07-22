### Q: Compare online RLHF, rejection sampling, RLAIF, constitutional feedback, and process supervision.
* **Difficulty:** Principal
* **Category:** Alignment
* **The 10-Second Pitch:** Online RLHF learns from fresh policy behavior; rejection sampling filters candidates; RLAIF uses AI preferences; constitutional feedback applies explicit principles; process supervision labels intermediate steps. They differ in data source, exploration, cost, and proxy risk.
* **The Deep Dive:** Online collection reduces off-policy mismatch but needs continual humans/reward models and safe exploration. Rejection sampling generates $n$ candidates, scores them, and SFTs on winners—simple but limited to sampled behavior and judge quality. RLAIF scales labels but transfers judge bias. Constitutional critique/revision makes rules explicit yet still requires policy interpretation and red-team validation. Process supervision can improve verifiable reasoning by rewarding intermediate correctness but needs granular trustworthy labels.
The 2025–26 counterpoint is outcome-verifiable reward: unit tests, math answer checkers, and format rewards give exact programmatic signals wherever correctness is machine-checkable. R1-style reasoning pipelines dropped learned process reward models for exactly this reason—a learned PRM is itself a proxy that the policy reward-hacks, step-level labels are costly and noisy, and running a per-step verifier at training scale is expensive. Process supervision still earns its cost where no terminal verifier exists or credit assignment across long chains dominates; when a cheap outcome verifier is available, it usually wins on robustness and price.

* **Production Reality & Tradeoffs:** Combine methods and keep independent human evaluation. Synthetic feedback can collapse diversity or amplify model blind spots. Process labels are domain-specific and expensive.
* **Red Flag:** Calling AI feedback objective or assuming more sampled candidates guarantee quality.

