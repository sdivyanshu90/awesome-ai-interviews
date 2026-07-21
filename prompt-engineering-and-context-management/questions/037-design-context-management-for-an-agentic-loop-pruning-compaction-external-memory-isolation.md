### Q: Design context management for an agentic loop: tool-result pruning, mid-loop compaction, external memory, and sub-agent isolation.
* **Difficulty:** Principal
* **Category:** Context Management
* **The 10-Second Pitch:** Rebuild the context every step under an explicit budget: stable system prompt and tool schemas first for caching, history appended strictly in order, tool results demoted through a pruning ladder, compaction only at checkpoints with constraints carried verbatim, large state spilled to files, and heavy exploration fanned out to sub-agents that return only conclusions.
* **The Deep Dive:** Per-step assembly and ordering. With window $C$, reserve output and margin first, then lay out: system prompt and tool schemas (stable, cacheable), then the transcript, newest tool results last. Append-only ordering is a cache requirement, not a style choice: prefix caches key on exact tokens, so mutating any earlier message invalidates every token after the edit point. A 200-step loop that rewrites history each step re-prefills the whole window 200 times; one that only appends pays prefill once per new segment. Therefore prune and compact at explicit checkpoints, accepting a single cache bust, instead of continuously rewriting.

  What to drop: tool results dominate agent context — in file-heavy coding loops they are commonly 70-90 percent of tokens. Use a demotion ladder per result: full text, then truncated head and tail with an elision marker, then a one-line reference such as "read 412 lines from config.py; re-read if needed", then dropped. Old tool results are the safest to clear because the environment is re-fetchable — the file system is the recoverable copy. Never demote: stated constraints, decisions with their reasons, open questions, and the most recent error message. Mid-loop compaction triggers near a threshold (say 80 percent of budget): summarize the oldest span into a structured note (goal, constraints, decisions, current state, pending work, touched paths) and keep recent turns raw. The canonical failure is constraint loss — "never touch the prod config" stated once at turn 3 vanishes from the turn-60 summary and the agent violates it — so constraints and user instructions get an exempt section carried verbatim through every compaction.

  ```mermaid
  flowchart TD
    A[Assemble step context] --> B{Over 80 percent of budget?}
    B -- no --> C[Model step]
    B -- yes --> D[Compact - carry constraints verbatim, summarize the rest]
    D --> C
    C --> E{Tool call}
    E -- heavy exploration --> F[Sub-agent with its own window]
    F --> G[Short report appended to main context]
    E -- ordinary --> H[Execute and append full result]
    H --> I[Demote older tool results to references]
    G --> A
    I --> A
  ```

  External memory and isolation. File-system-as-memory lets the agent write plans, notes, and todo lists to files and re-read them on demand; re-appending the todo list also recites goals into the recency window, countering lost-in-the-middle drift on long tasks. Sub-agent isolation protects the orchestrator: a search or analysis task runs in a child context with its own window, and only its final 1-2k-token report enters the parent — a 100k-token exploration, or a poisoned webpage, never touches the main transcript. Falsifiable test: plant $K$ explicit constraints early, run the loop long enough to force at least two compactions, then probe both recall of the constraints and behavioral compliance; any retention below 100 percent falsifies the compactor. Separately, chart cache-read tokens per step — a sudden drop toward zero reveals accidental history mutation.
* **Production Reality & Tradeoffs:** Compaction is where agents silently lose the plot; track task success as a function of compaction count. Over-aggressive pruning causes re-fetch loops (the agent re-reads the same file five times, spending what pruning saved). Sub-agents add latency and duplicate prefill cost, so reserve them for genuinely heavy or untrusted work. Log per-step token composition by segment or you cannot debug any of this.
* **Red Flag:** Summarizing the whole history uniformly — active constraints and the current plan included — or mutating messages in place every step, which destroys correctness and cache hit rate in one move.
