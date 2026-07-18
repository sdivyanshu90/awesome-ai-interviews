# 11. System Design

Thirty-five modular principal-level design profiles covering architecture and capacity fundamentals, end-to-end AI products, shared platforms, economics, security, reliability, migrations, production readiness, and incident diagnosis.

[Back to the complete syllabus](../SYLLABUS.md)

## Architecture and capacity fundamentals

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 001 | [Turn an ambiguous AI product request into functional requirements, SLOs, risks, and evaluation criteria.](questions/001-turn-an-ambiguous-ai-product-request-into-functional-requirements-slos-risks-and-evaluatio.md) | Senior | Requirements |
| 002 | [Estimate QPS, tokens/s, GPU count, KV memory, storage, network, queues, and monthly cost.](questions/002-estimate-qps-tokens-s-gpu-count-kv-memory-storage-network-queues-and-monthly-cost.md) | Principal | Capacity Planning |
| 003 | [Apply Little’s Law and queueing intuition to p50/p95/p99, burst traffic, and head-of-line blocking.](questions/003-apply-little-s-law-and-queueing-intuition-to-p50-p95-p99-burst-traffic-and-head-of-line-bl.md) | Principal | Queueing |
| 004 | [Balance latency, throughput, quality, reliability, safety, privacy, and cost under explicit priorities.](questions/004-balance-latency-throughput-quality-reliability-safety-privacy-and-cost-under-explicit-prio.md) | Principal | Architecture |
| 005 | [Choose synchronous, streaming, asynchronous, batch, event-driven, or human-in-the-loop execution.](questions/005-choose-synchronous-streaming-asynchronous-batch-event-driven-or-human-in-the-loop-executio.md) | Principal | Architecture |
| 006 | [Design idempotency, retries, timeouts, circuit breakers, backpressure, dead letters, and compensation.](questions/006-design-idempotency-retries-timeouts-circuit-breakers-backpressure-dead-letters-and-compens.md) | Principal | Reliability |
| 007 | [Define SLIs, SLOs, error budgets, degraded modes, incident ownership, and rollback.](questions/007-define-slis-slos-error-budgets-degraded-modes-incident-ownership-and-rollback.md) | Principal | SRE |

## End-to-end product designs

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 008 | [Design a streaming chat assistant with memory, RAG, tools, safety, observability, and citations.](questions/008-design-a-streaming-chat-assistant-with-memory-rag-tools-safety-observability-and-citations.md) | Principal | System Design |
| 009 | [Design a multi-tenant enterprise knowledge assistant with ACLs, versions, deletion, and audit.](questions/009-design-a-multi-tenant-enterprise-knowledge-assistant-with-acls-versions-deletion-and-audit.md) | Principal | System Design |
| 010 | [Design a customer-support copilot with CRM, policy, action tools, approval, and escalation.](questions/010-design-a-customer-support-copilot-with-crm-policy-action-tools-approval-and-escalation.md) | Principal | System Design |
| 011 | [Design a coding copilot with repository indexing, context selection, sandboxing, tests, and patches.](questions/011-design-a-coding-copilot-with-repository-indexing-context-selection-sandboxing-tests-and-pa.md) | Principal | System Design |
| 012 | [Design a legal or medical assistant with evidence, abstention, privacy, and expert review.](questions/012-design-a-legal-or-medical-assistant-with-evidence-abstention-privacy-and-expert-review.md) | Principal | High-Stakes AI |
| 013 | [Design a real-time voice assistant with streaming ASR, dialogue, tools, TTS, and barge-in.](questions/013-design-a-real-time-voice-assistant-with-streaming-asr-dialogue-tools-tts-and-barge-in.md) | Principal | System Design |
| 014 | [Design a multimodal document platform for PDFs, tables, images, extraction, and human verification.](questions/014-design-a-multimodal-document-platform-for-pdfs-tables-images-extraction-and-human-verifica.md) | Principal | System Design |
| 015 | [Design a recommendation/ranking system with retrieval, features, exploration, feedback, and safety.](questions/015-design-a-recommendation-ranking-system-with-retrieval-features-exploration-feedback-and-sa.md) | Principal | ML System Design |
| 016 | [Design content moderation with rules, classifiers, LLM review, appeals, and adversarial monitoring.](questions/016-design-content-moderation-with-rules-classifiers-llm-review-appeals-and-adversarial-monito.md) | Principal | Safety System Design |
| 017 | [Design an autonomous incident-response system with alerts, runbooks, tools, approvals, and postmortems.](questions/017-design-an-autonomous-incident-response-system-with-alerts-runbooks-tools-approvals-and-pos.md) | Principal | Agent System Design |

## Shared AI platforms

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 018 | [Design a model-routing gateway by quality, capability, latency, cost, privacy, and availability.](questions/018-design-a-model-routing-gateway-by-quality-capability-latency-cost-privacy-and-availability.md) | Principal | Platform Architecture |
| 019 | [Design a semantic/prefix cache with similarity, isolation, invalidation, freshness, and fallbacks.](questions/019-design-a-semantic-prefix-cache-with-similarity-isolation-invalidation-freshness-and-fallba.md) | Principal | Caching |
| 020 | [Design an evaluation platform for datasets, prompts, models, judges, humans, experiments, and gates.](questions/020-design-an-evaluation-platform-for-datasets-prompts-models-judges-humans-experiments-and-ga.md) | Principal | Platform Architecture |
| 021 | [Design a training-data platform for ingestion, lineage, deduplication, labeling, governance, and deletion.](questions/021-design-a-training-data-platform-for-ingestion-lineage-deduplication-labeling-governance-an.md) | Principal | Data Platform |
| 022 | [Design an embedding platform with online/offline consistency, versions, backfills, and migration.](questions/022-design-an-embedding-platform-with-online-offline-consistency-versions-backfills-and-migrat.md) | Principal | Embedding Platform |
| 023 | [Design a fine-tuning platform with secure data, adapters, evaluation, registry, serving, and rollback.](questions/023-design-a-fine-tuning-platform-with-secure-data-adapters-evaluation-registry-serving-and-ro.md) | Principal | ML Platform |
| 024 | [Design an agent platform with tools, policy, durable state, memory, tracing, simulation, and approval.](questions/024-design-an-agent-platform-with-tools-policy-durable-state-memory-tracing-simulation-and-app.md) | Principal | Agent Platform |
| 025 | [Design a multi-region inference platform with heterogeneous GPUs, quotas, failover, and cost control.](questions/025-design-a-multi-region-inference-platform-with-heterogeneous-gpus-quotas-failover-and-cost.md) | Principal | Infrastructure |

## Security, economics, reliability, and migration

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 026 | [Compute token economics, cache savings, GPU amortization, routing cost, and total cost per successful task.](questions/026-compute-token-economics-cache-savings-gpu-amortization-routing-cost-and-total-cost-per-suc.md) | Principal | Economics |
| 027 | [Decide between API, self-hosting, open weights, distillation, quantization, or a smaller specialist model.](questions/027-decide-between-api-self-hosting-open-weights-distillation-quantization-or-a-smaller-specia.md) | Principal | Strategy |
| 028 | [Secure identity, secrets, network, tenant boundaries, data residency, logging, models, and dependencies.](questions/028-secure-identity-secrets-network-tenant-boundaries-data-residency-logging-models-and-depend.md) | Principal | Security Architecture |
| 029 | [Design graceful degradation when retrieval, tools, policy, model providers, GPUs, or regions fail.](questions/029-design-graceful-degradation-when-retrieval-tools-policy-model-providers-gpus-or-regions-fa.md) | Principal | Reliability |
| 030 | [Migrate model providers, prompts, tokenizers, embeddings, indexes, schemas, and adapters without downtime.](questions/030-migrate-model-providers-prompts-tokenizers-embeddings-indexes-schemas-and-adapters-without.md) | Principal | Migration |
| 031 | [Conduct a production-readiness review covering capability, safety, operations, governance, and rollback.](questions/031-conduct-a-production-readiness-review-covering-capability-safety-operations-governance-and.md) | Principal | Production Readiness |
| 032 | [Diagnose quality loss after a provider/model upgrade using replay, shadowing, and stage attribution.](questions/032-diagnose-quality-loss-after-a-provider-model-upgrade-using-replay-shadowing-and-stage-attr.md) | Principal | Debugging |
| 033 | [Diagnose acceptable mean latency but failed p99 under long-context and mixed-priority traffic.](questions/033-diagnose-acceptable-mean-latency-but-failed-p99-under-long-context-and-mixed-priority-traf.md) | Principal | Performance Debugging |
| 034 | [Diagnose a cost explosion caused by context growth, retry storms, poor cache reuse, or agent loops.](questions/034-diagnose-a-cost-explosion-caused-by-context-growth-retry-storms-poor-cache-reuse-or-agent.md) | Principal | Cost Debugging |
| 035 | [Communicate architecture trade-offs and ownership to product, security, legal, finance, and infrastructure.](questions/035-communicate-architecture-trade-offs-and-ownership-to-product-security-legal-finance-and-in.md) | Principal | Leadership |

## Architecture review lens

```text
requirements + risks + workload
             |
             v
  data plane / control plane / trust boundaries
             |
             v
quality + safety + SLO + capacity + cost evidence
             |
             v
 rollout -> observability -> degradation -> rollback
             |
             v
      named operational owners
```

## Primary references

- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST Generative AI Profile](https://doi.org/10.6028/NIST.AI.600-1)
- [Google SRE Workbook](https://sre.google/workbook/table-of-contents/)
- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/)

