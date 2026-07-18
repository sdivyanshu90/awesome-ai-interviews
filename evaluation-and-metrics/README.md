# 10. Evaluation & Metrics

Thirty-six modular interview profiles covering predictive statistics, generation and retrieval metrics, RAG and hallucination evaluation, LLM judges, human evaluation, agents and multimodal systems, safety/fairness, and production evaluation platforms.

[Back to the complete syllabus](../SYLLABUS.md)

## Statistical and predictive evaluation

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 001 | [Derive accuracy, precision, recall, specificity, F1, ROC-AUC, PR-AUC, log loss, and Brier score.](questions/001-derive-accuracy-precision-recall-specificity-f1-roc-auc-pr-auc-log-loss-and-brier-score.md) | Mid | Math |
| 002 | [Explain macro, micro, weighted, per-class, multilabel, and threshold-dependent aggregation.](questions/002-explain-macro-micro-weighted-per-class-multilabel-and-threshold-dependent-aggregation.md) | Senior | Evaluation |
| 003 | [Why is ROC-AUC misleading under severe imbalance, and how should thresholds be selected?](questions/003-why-is-roc-auc-misleading-under-severe-imbalance-and-how-should-thresholds-be-selected.md) | Senior | Evaluation |
| 004 | [Measure calibration using reliability diagrams, ECE variants, NLL, Brier score, and selective risk.](questions/004-measure-calibration-using-reliability-diagrams-ece-variants-nll-brier-score-and-selective.md) | Senior | Calibration |
| 005 | [Compare models using paired bootstrap, permutation tests, confidence intervals, power, and sequential tests.](questions/005-compare-models-using-paired-bootstrap-permutation-tests-confidence-intervals-power-and-seq.md) | Principal | Statistics |

## Language generation metrics

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 006 | [Derive BLEU, brevity penalty, smoothing, and its corpus/sentence-level limitations.](questions/006-derive-bleu-brevity-penalty-smoothing-and-its-corpus-sentence-level-limitations.md) | Senior | NLP Metrics |
| 007 | [Compare ROUGE-N, ROUGE-L, METEOR, chrF, CIDEr, BERTScore, and learned metrics.](questions/007-compare-rouge-n-rouge-l-meteor-chrf-cider-bertscore-and-learned-metrics.md) | Senior | NLP Metrics |
| 008 | [Derive perplexity and explain tokenizer, domain, length, and contamination caveats.](questions/008-derive-perplexity-and-explain-tokenizer-domain-length-and-contamination-caveats.md) | Senior | Math |
| 009 | [Evaluate summarization for factuality, coverage, compression, style, and citation support.](questions/009-evaluate-summarization-for-factuality-coverage-compression-style-and-citation-support.md) | Senior | Evaluation |
| 010 | [Evaluate translation for adequacy, fluency, terminology, gender, dialect, and low-resource robustness.](questions/010-evaluate-translation-for-adequacy-fluency-terminology-gender-dialect-and-low-resource-robu.md) | Senior | Evaluation |
| 011 | [Evaluate code with exact match, compilation, unit tests, execution, pass@k, mutation tests, and security scans.](questions/011-evaluate-code-with-exact-match-compilation-unit-tests-execution-pass-k-mutation-tests-and.md) | Senior | Code Evaluation |

## RAG, hallucination, and grounding evaluation

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 012 | [Evaluate retrieval using Recall@k, MRR, MAP, nDCG, hit rate, and graded/multi-hop relevance.](questions/012-evaluate-retrieval-using-recall-k-mrr-map-ndcg-hit-rate-and-graded-multi-hop-relevance.md) | Senior | Information Retrieval |
| 013 | [Explain RAGAS context precision, context recall, faithfulness, and answer relevance.](questions/013-explain-ragas-context-precision-context-recall-faithfulness-and-answer-relevance.md) | Senior | RAG Evaluation |
| 014 | [Validate RAGAS or other synthetic metrics against expert judgments and controlled perturbations.](questions/014-validate-ragas-or-other-synthetic-metrics-against-expert-judgments-and-controlled-perturba.md) | Principal | Metric Validation |
| 015 | [Distinguish hallucination, unsupported claim, contradiction, stale fact, citation error, and omission.](questions/015-distinguish-hallucination-unsupported-claim-contradiction-stale-fact-citation-error-and-om.md) | Principal | Taxonomy |
| 016 | [Detect hallucinations when ground truth is incomplete, conflicting, time-sensitive, or unavailable.](questions/016-detect-hallucinations-when-ground-truth-is-incomplete-conflicting-time-sensitive-or-unavai.md) | Principal | Hallucination Evaluation |
| 017 | [Evaluate citation completeness, correctness, entailment, source quality, and span accuracy.](questions/017-evaluate-citation-completeness-correctness-entailment-source-quality-and-span-accuracy.md) | Principal | Grounding Evaluation |

## LLM judges and human evaluation

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 018 | [Design an LLM-as-a-judge rubric with atomic criteria, evidence, and calibrated scales.](questions/018-design-an-llm-as-a-judge-rubric-with-atomic-criteria-evidence-and-calibrated-scales.md) | Senior | LLM-as-a-Judge |
| 019 | [Diagnose position, verbosity, style, self-preference, reference, contamination, and prompt biases in judges.](questions/019-diagnose-position-verbosity-style-self-preference-reference-contamination-and-prompt-biase.md) | Principal | LLM-as-a-Judge |
| 020 | [Validate judges using agreement, correlation, calibration, perturbation, and adjudicated human sets.](questions/020-validate-judges-using-agreement-correlation-calibration-perturbation-and-adjudicated-human.md) | Principal | Metric Validation |
| 021 | [Compare pointwise, pairwise, listwise, tournament, Elo, Bradley–Terry, and Plackett–Luce evaluation.](questions/021-compare-pointwise-pairwise-listwise-tournament-elo-bradley-terry-and-plackett-luce-evaluat.md) | Senior | Preference Modeling |
| 022 | [Design human labeling with qualification, blind review, ties, disagreement, audit, and quality control.](questions/022-design-human-labeling-with-qualification-blind-review-ties-disagreement-audit-and-quality.md) | Principal | Human Evaluation |

## Agent, multimodal, safety, and fairness evaluation

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 023 | [Evaluate agents for success, trajectory validity, tool correctness, recovery, safety, cost, and latency.](questions/023-evaluate-agents-for-success-trajectory-validity-tool-correctness-recovery-safety-cost-and.md) | Principal | Agent Evaluation |
| 024 | [Build simulation and adversarial environments without overfitting to deterministic fixtures.](questions/024-build-simulation-and-adversarial-environments-without-overfitting-to-deterministic-fixture.md) | Principal | Simulation |
| 025 | [Evaluate long-context retrieval, instruction retention, distractors, cross-document reasoning, and latency.](questions/025-evaluate-long-context-retrieval-instruction-retention-distractors-cross-document-reasoning.md) | Principal | Long-Context Evaluation |
| 026 | [Evaluate VLMs for OCR, grounding, spatial reasoning, counting, hallucination, and real-camera shift.](questions/026-evaluate-vlms-for-ocr-grounding-spatial-reasoning-counting-hallucination-and-real-camera-s.md) | Principal | Multimodal Evaluation |
| 027 | [Evaluate generated images/audio for fidelity, alignment, diversity, memorization, safety, and preference.](questions/027-evaluate-generated-images-audio-for-fidelity-alignment-diversity-memorization-safety-and-p.md) | Principal | Generative Media Evaluation |
| 028 | [Evaluate prompt injection, jailbreak, sensitive disclosure, excessive agency, and unsafe tool use.](questions/028-evaluate-prompt-injection-jailbreak-sensitive-disclosure-excessive-agency-and-unsafe-tool.md) | Principal | Safety Evaluation |
| 029 | [Evaluate fairness across demographic, language, dialect, geography, accessibility, and intersectional slices.](questions/029-evaluate-fairness-across-demographic-language-dialect-geography-accessibility-and-intersec.md) | Principal | Fairness Evaluation |

## Evaluation platforms and online experiments

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 030 | [Build golden, adversarial, counterfactual, metamorphic, synthetic, and production-replay datasets.](questions/030-build-golden-adversarial-counterfactual-metamorphic-synthetic-and-production-replay-datase.md) | Principal | Dataset Engineering |
| 031 | [Prevent test leakage, benchmark contamination, repeated tuning, judge gaming, and metric Goodharting.](questions/031-prevent-test-leakage-benchmark-contamination-repeated-tuning-judge-gaming-and-metric-goodh.md) | Principal | Evaluation Integrity |
| 032 | [Design an evaluation harness with immutable inputs, versioned artifacts, reproducibility, and release gates.](questions/032-design-an-evaluation-harness-with-immutable-inputs-versioned-artifacts-reproducibility-and.md) | Principal | Evaluation Platform |
| 033 | [Run online experiments with interference, novelty, delayed outcomes, guardrails, and heterogeneous effects.](questions/033-run-online-experiments-with-interference-novelty-delayed-outcomes-guardrails-and-heterogen.md) | Principal | Online Experimentation |
| 034 | [Diagnose offline improvements with declining production satisfaction or business outcomes.](questions/034-diagnose-offline-improvements-with-declining-production-satisfaction-or-business-outcomes.md) | Principal | Debugging |
| 035 | [Construct a scorecard balancing capability, reliability, safety, latency, cost, and environmental impact.](questions/035-construct-a-scorecard-balancing-capability-reliability-safety-latency-cost-and-environment.md) | Principal | Decision Framework |
| 036 | [Implement bootstrap model comparison, retrieval metrics, calibration metrics, and judge-bias tests.](questions/036-implement-bootstrap-model-comparison-retrieval-metrics-calibration-metrics-and-judge-bias.md) | Mid | Coding |

## Primary references

- [RAGAS: Automated Evaluation of Retrieval Augmented Generation](https://arxiv.org/abs/2309.15217)
- [BLEU: a Method for Automatic Evaluation of Machine Translation](https://aclanthology.org/P02-1040/)
- [BERTScore: Evaluating Text Generation with BERT](https://arxiv.org/abs/1904.09675)
- [Evaluating Large Language Models Trained on Code](https://arxiv.org/abs/2107.03374)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)

