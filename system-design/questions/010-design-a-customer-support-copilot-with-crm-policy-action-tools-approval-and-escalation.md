### Q: Design a customer-support copilot with CRM, policy, action tools, approval, and escalation.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Ground recommendations in versioned customer and policy data, distinguish drafts from commits, authorize each action against the agent/user, bind approvals, and escalate with a complete evidence-backed handoff.
* **The Deep Dive:** Authenticate support agent and customer context; retrieve CRM fields under field-level policy and fetch effective-dated product/refund procedures. Model proposes a cited response and structured next action. Deterministic rules validate eligibility, amount, account ownership, channel, and separation of duties. Low-risk drafts may auto-populate; refunds, cancellations, credits, or outbound messages require role/risk-based approval bound to canonical arguments. Executor uses scoped JIT credentials and idempotency. Conversation and tool state are durable so reconnect/retry cannot duplicate effects. Escalation packages summary, customer intent, evidence, attempted actions, policy conflicts, confidence, and audit trail, while allowing the human to inspect originals.
* **Production Reality & Tradeoffs:** CRM data is stale/incomplete and policies conflict. Overautomation harms trust; excessive approval adds handle time. Measure resolution, correction, policy violations, customer satisfaction, action reversals, latency, and subgroup outcomes.
* **Red Flag:** Letting the LLM decide refund eligibility or write directly to CRM because the support agent is authenticated.

