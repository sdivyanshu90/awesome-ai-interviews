### Q: Explain ReAct’s reasoning-action-observation loop, state update, and termination.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** ReAct (Yao et al., 2022) interleaves free-text thoughts, actions, and observations in one shared prompt trace so evidence from tools grounds subsequent reasoning; production harnesses then add typed actions, validation, budgets, and explicit termination on top of that loop.
* **The Deep Dive:** In the paper, the trajectory is a single shared visible context: the model appends a free-text thought, then an action such as `Search[Apple Remote]`, and the environment appends an observation; every subsequent step conditions on the whole trace. Thoughts are not a private decision state—the next step reads them in the same prompt everything else lives in. Actions were free text parsed out of prose against a small informal action set (Wikipedia search/lookup/finish, ALFWorld and WebShop commands), with no typed action space, no argument validation, and no budget enforcement; an episode ends when the model emits a finish action or hits a step cap.

  ```text
  paper:     shared trace = (thought → action → observation)* → finish[answer]
  hardened:  state → typed action → validate/authorize → sandboxed tool → bounded observation
  ```
  The paper's contribution is the interleaving itself: acting grounds reasoning in fresh observations rather than hallucinated recall, and reasoning steers what to look up next, beating act-only and CoT-only baselines on HotpotQA, FEVER, ALFWorld, and WebShop while reducing hallucination. Production harnesses add what the paper never had: typed tool schemas, orchestrator-side parsing, validation, and authorization of each action, per-run budgets, explicit termination on success, infeasibility, denial, or budget exhaustion, and treatment of observations as untrusted quoted data—never higher-priority instructions.
  ```mermaid
  flowchart LR
  S[Shared trace] --> P[Thought + action]
  P --> V{Harness validates}
  V -->|allowed| T[Tool]
  T --> O[Observation appended]
  O --> S
  V -->|denied/error| R[Recover or stop]
  ```
  The hardened loop keeps the paper's reason-act-observe alternation but relocates parsing, authority, and stopping from the prompt into the orchestrator.

* **Production Reality & Tradeoffs:** Explicit structured state and tool calls are easier to replay than hidden free-form traces. Do not permit unrestricted shell, browser, or financial actions merely because the model generated syntactically valid arguments.
* **Red Flag:** Giving a ReAct prompt direct production credentials and calling it guardrailed.
