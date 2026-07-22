### Q: Design a support agent with policy retrieval, account tools, refunds, approval, and escalation.
* **Difficulty:** Principal
* **Category:** System Design
* **The 10-Second Pitch:** Use a durable state graph: authenticate and resolve account scope; retrieve authorized policy with citations; collect facts via read tools; compute eligibility in deterministic policy code; preview any refund; require approval where required; execute idempotently; and log the complete trace for audit.
* **The Deep Dive:** The LLM handles language understanding, clarification, and explanation—not refund authorization. Policy retrieval supplies current wording, while deterministic rules enforce dates, entitlement, and monetary limits. A human interrupt receives the evidence, proposed amount, reason, and policy version; a post-action node confirms the ledger state before replying.

  ```text
  verify identity → retrieve policy + inspect account → deterministic eligibility
       → [clarify | deny with citation | approval] → idempotent refund → confirm + audit
  ```
  Policy text is versioned retrieval evidence; account data comes through read-only scoped tools. A deterministic eligibility service computes refund limits, while the model explains and proposes. Refund execution requires an idempotency key, exact preview, current account version, and approval above threshold. Unsupported policy, conflicting records, or user identity uncertainty routes to a human with the evidence bundle.

* **Production Reality & Tradeoffs:** Prevent duplicate refunds under retries, redact payment data in traces, and retain a safe fallback when tools are unavailable. Track escalation quality and policy-citation correctness, not only resolution rate.
* **Red Flag:** Allowing the model to decide refund eligibility from policy prose and directly call a payment API.
