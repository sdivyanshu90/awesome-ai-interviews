### Q: Explain LLM06 Excessive Agency and enforce authorization independently of the model.
* **Difficulty:** Senior
* **Category:** OWASP
* **The 10-Second Pitch:** Excessive agency occurs when the model has more capabilities, permissions, autonomy, or impact than required. The executor—not model reasoning—authenticates and authorizes each action.
* **The Deep Dive:** Expose minimal tools, split read/write, use scoped short-lived credentials, set amount/resource/time limits, require previews/approval for consequential actions, and enforce idempotency/budgets. Policy uses actual principal/resource/state. Disable arbitrary tool chaining and egress. Audit and provide kill switch/compensation.
* **Production Reality & Tradeoffs:** Human-in-loop can become rubber stamping; show exact evidence/action. Least privilege may reduce convenience but bounds injection impact.
Excessive agency combines broad capabilities, excessive permissions, insufficient approval, and long autonomous horizons. Reduce the action space and credential scope; separate read/propose/execute roles; impose budgets and stop conditions; require preview for material effects; and reauthorize against current state. The executor checks identity and resource ownership independently. A model’s statement that the user approved an action is untrusted input, not authorization evidence.

* **Red Flag:** Giving an agent administrator credentials and asking it to be careful.
