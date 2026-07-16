### Q: How should prompts express uncertainty, abstention, clarification, and conflicting-evidence handling?
* **Difficulty:** Principal
* **Category:** Prompt Engineering
* **The 10-Second Pitch:** Define observable decision states and evidence obligations: answer with citations when sufficient, ask a minimal clarifying question when ambiguity is resolvable, report conflicts, or abstain when evidence/authority is inadequate—without inventing numeric confidence.
* **The Deep Dive:** Specify what counts as sufficient evidence, which sources/versions outrank others, and which missing fields materially change the result. Require atomic claim citations and statuses such as `supported`, `conflicting`, `insufficient`, or `outside_scope`. Clarification asks the highest-value question whose answer changes action; cap rounds and offer safe defaults only where authorized. Conflict output lists each claim/source/date and avoids arbitrary synthesis. Abstention states what cannot be determined and what evidence/tool/human could resolve it.

```json
{"status":"conflicting_evidence","answer":null,
 "conflicts":[{"claim":"...","sources":["s1:p3","s2:p8"]}],
 "next_step":"Confirm effective policy version"}
```

Do not ask the model for a bare confidence percentage: it is generally uncalibrated. Build confidence from retrieval coverage, verifier results, data freshness, known model calibration, and task risk, then choose answer/review/abstain externally. Test unanswerable, ambiguous, adversarial, and source-conflict cases plus benign cases to measure over-abstention.
* **Production Reality & Tradeoffs:** More abstention improves safety but lowers task completion and may disproportionately affect low-resource languages. Clarification adds turns/latency. Provide users actionable, nondeceptive uncertainty without overwhelming them.
* **Red Flag:** Instructing “if unsure, say so” with no criteria, or forcing a single answer when sources conflict.

