### Q: Compare tree-of-thought, graph search, beam reasoning, MCTS, verifiers, and program execution.
* **Difficulty:** Principal
* **Category:** Reasoning
* **The 10-Second Pitch:** Search methods generate and select reasoning states under a budget; their value depends more on state representation, branching, verifier calibration, and executable grounding than on the search label.
* **The Deep Dive:** Tree-of-thought samples candidate next thoughts, scores, and expands/prunes. Graph search can merge equivalent states and avoid duplicate subproblems if a canonical state exists. Beam keeps top $b$ partial paths by heuristic but loses diversity and can discard a necessary low-score prefix. MCTS balances exploration/exploitation through selection, expansion, rollout/evaluation, and backup; LLM priors/value estimates are often poorly calibrated and environments nonstationary. Verifiers may be outcome tests, process reward models, constraints, or judges. Program execution turns a proposed state into deterministic code/solver output, providing stronger feedback but requiring sandbox and a formalizable task.

```text
proposal model -> candidate states -> verifier/tool -> search policy -> terminal proof/result
                         ^                |
                         +--- feedback ---+
```

Deduplicate, cap nodes/tokens/time/cost, preserve provenance, and separate final answer from search traces. Evaluate success per compute, verifier false acceptance, diversity, and robustness to misleading states.
* **Production Reality & Tradeoffs:** Search multiplies inference and correlated candidates do not provide independent evidence. Weak judges reward verbosity. Use calculators/solvers/tests when available; direct answers win on simple tasks.
* **Red Flag:** Assuming MCTS guarantees correctness without a trustworthy state transition and verifier.

