# 5. Fine-Tuning & Model Alignment

Every syllabus question has one independent Markdown answer file. The index separates question discovery from answer content.

| # | Question | Difficulty | Category | Answer |
|---:|---|---|---|---|
| 1 | Choose among prompting, RAG, continued pretraining, SFT, PEFT, and full tuning from failure evidence. | Principal | Strategy | [Open answer](questions/001-choose-among-prompting-rag-continued-pretraining-sft-peft-and-full-tuning-from-failure-evi.md) |
| 2 | Derive the SFT objective, shifted labels, response-only masking, chat templates, and sequence packing. | Senior | Deep Learning | [Open answer](questions/002-derive-the-sft-objective-shifted-labels-response-only-masking-chat-templates-and-sequence.md) |
| 3 | Compare full tuning, layer freezing, adapters, prompt/prefix tuning, IA3, BitFit, and LoRA. | Senior | Architecture | [Open answer](questions/003-compare-full-tuning-layer-freezing-adapters-prompt-prefix-tuning-ia3-bitfit-and-lora.md) |
| 4 | Design a licensed, provenance-tracked, deduplicated, contamination-resistant SFT dataset. | Principal | Data | [Open answer](questions/004-design-a-licensed-provenance-tracked-deduplicated-contamination-resistant-sft-dataset.md) |
| 5 | Diagnose memorization, template overfitting, paraphrase failure, and catastrophic forgetting. | Senior | Training | [Open answer](questions/005-diagnose-memorization-template-overfitting-paraphrase-failure-and-catastrophic-forgetting.md) |
| 6 | Derive Bradley–Terry reward modeling and explain reward-score identifiability. | Senior | Math | [Open answer](questions/006-derive-bradley-terry-reward-modeling-and-explain-reward-score-identifiability.md) |
| 7 | Explain RLHF end to end: SFT, reward model, rollouts, value model, PPO, KL, and iteration. | Principal | Alignment | [Open answer](questions/007-explain-rlhf-end-to-end-sft-reward-model-rollouts-value-model-ppo-kl-and-iteration.md) |
| 8 | Derive DPO from KL-regularized reward maximization and reference-relative log odds. | Principal | Math | [Open answer](questions/008-derive-dpo-from-kl-regularized-reward-maximization-and-reference-relative-log-odds.md) |
| 9 | Diagnose reward hacking, overoptimization, mode collapse, verbosity bias, and policy drift. | Principal | Safety | [Open answer](questions/009-diagnose-reward-hacking-overoptimization-mode-collapse-verbosity-bias-and-policy-drift.md) |
| 10 | Align helpfulness, honesty, harmlessness, style, tool reliability, and refusal calibration jointly. | Principal | System Design | [Open answer](questions/010-align-helpfulness-honesty-harmlessness-style-tool-reliability-and-refusal-calibration-join.md) |
| 11 | Serve, batch, cache, secure, merge, and roll back thousands of tenant adapters. | Principal | LLMOps | [Open answer](questions/011-serve-batch-cache-secure-merge-and-roll-back-thousands-of-tenant-adapters.md) |
| 12 | Fine-tune inside a regulated boundary with lineage, residency, deletion, and memorization controls. | Principal | System Design | [Open answer](questions/012-fine-tune-inside-a-regulated-boundary-with-lineage-residency-deletion-and-memorization-con.md) |
| 13 | Evaluate target gains against retained reasoning, multilingual, safety, calibration, and tool-use capabilities. | Principal | Evaluation | [Open answer](questions/013-evaluate-target-gains-against-retained-reasoning-multilingual-safety-calibration-and-tool.md) |
| 14 | Derive $W'=W+(\alpha/r)BA$, initialization, scaling, rank, dropout, and merge semantics. | Senior | Math | [Open answer](questions/014-derive-initialization-scaling-rank-dropout-and-merge-semantics.md) |
| 15 | Choose LoRA target modules, rank patterns, learning rates, and adapter composition strategies. | Principal | Training | [Open answer](questions/015-choose-lora-target-modules-rank-patterns-learning-rates-and-adapter-composition-strategies.md) |
| 16 | Explain QLoRA, NF4, double quantization, paged optimizers, and gradient flow through frozen weights. | Senior | Training | [Open answer](questions/016-explain-qlora-nf4-double-quantization-paged-optimizers-and-gradient-flow-through-frozen-we.md) |
| 17 | Design preference collection with pairwise labels, rankings, ties, disagreement, and annotator calibration. | Senior | Data | [Open answer](questions/017-design-preference-collection-with-pairwise-labels-rankings-ties-disagreement-and-annotator.md) |
| 18 | Derive PPO’s clipped surrogate, advantage estimation, value loss, entropy, and KL control. | Principal | Math | [Open answer](questions/018-derive-ppo-s-clipped-surrogate-advantage-estimation-value-loss-entropy-and-kl-control.md) |
| 19 | Compare online RLHF, rejection sampling, RLAIF, constitutional feedback, and process supervision. | Principal | Alignment | [Open answer](questions/019-compare-online-rlhf-rejection-sampling-rlaif-constitutional-feedback-and-process-supervisi.md) |
| 20 | Compare DPO, IPO, KTO, ORPO, SimPO, CPO, and online preference optimization. | Principal | Alignment | [Open answer](questions/020-compare-dpo-ipo-kto-orpo-simpo-cpo-and-online-preference-optimization.md) |
| 21 | Explain preference length bias, reference choice, beta, label noise, and off-policy limitations. | Principal | Alignment | [Open answer](questions/021-explain-preference-length-bias-reference-choice-beta-label-noise-and-off-policy-limitation.md) |
| 22 | Build adversarial and human evaluation gates for an aligned-model release. | Principal | Evaluation | [Open answer](questions/022-build-adversarial-and-human-evaluation-gates-for-an-aligned-model-release.md) |
| 23 | Implement LoRA injection, response masking, DPO loss, and adapter checkpoint validation. | Mid | Coding | [Open answer](questions/023-implement-lora-injection-response-masking-dpo-loss-and-adapter-checkpoint-validation.md) |
| 24 | Derive GRPO: group-relative advantages, removing the value model, KL treatment, and known biases versus PPO and RLOO. | Principal | Math | [Open answer](questions/024-derive-grpo-group-relative-advantages-removing-the-value-model-kl-treatment-and-biases.md) |
| 25 | Design an RLVR pipeline: verifiable rewards, reward shaping, entropy and length dynamics, and elicitation versus capability creation. | Principal | Alignment | [Open answer](questions/025-design-an-rlvr-pipeline-verifiable-rewards-shaping-entropy-length-and-elicit-vs-create.md) |
| 26 | Compare outcome reward models, process reward models, and rule-based verifiers for reasoning training. | Senior | Alignment | [Open answer](questions/026-compare-outcome-process-and-rule-based-verifier-rewards-for-reasoning-training.md) |
| 27 | Explain LLM distillation as post-training: teacher-trace SFT, logit distillation, forward versus reverse KL, and on-policy GKD. | Senior | Training | [Open answer](questions/027-explain-llm-distillation-teacher-trace-sft-logit-kl-forward-vs-reverse-and-on-policy-gkd.md) |
| 28 | Design a synthetic SFT data pipeline: self-instruct, Evol-Instruct, rejection sampling, and defenses against collapse and bias transfer. | Senior | Data | [Open answer](questions/028-design-a-synthetic-sft-data-pipeline-with-defenses-against-collapse-and-bias-transfer.md) |
| 29 | Compare model merging: task arithmetic, SLERP, TIES and DARE, and model soups — when merging beats multi-task SFT. | Senior | Architecture | [Open answer](questions/029-compare-model-merging-task-arithmetic-slerp-ties-dare-and-model-soups.md) |
| 30 | Why does fine-tuning even on benign data erode safety alignment, and how do you defend a fine-tuning API? | Principal | Safety | [Open answer](questions/030-explain-why-benign-fine-tuning-erodes-safety-alignment-and-defend-a-fine-tuning-api.md) |
| 31 | Execute continued pretraining: data-mixture replay, learning-rate re-warming, tokenizer extension, and stopping criteria. | Senior | Training | [Open answer](questions/031-execute-continued-pretraining-replay-lr-rewarming-tokenizer-extension-and-stopping.md) |
| 32 | Extend reward modeling beyond Bradley–Terry: generative and LLM-judge reward models, margin objectives, and evaluating the reward model itself. | Senior | Alignment | [Open answer](questions/032-extend-reward-modeling-beyond-bradley-terry-generative-judges-margins-and-rm-evaluation.md) |

**Implemented question count:** 32

[Back to complete syllabus](../SYLLABUS.md)

