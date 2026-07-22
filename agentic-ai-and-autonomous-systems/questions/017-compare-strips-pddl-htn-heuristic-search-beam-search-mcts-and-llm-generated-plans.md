### Q: Compare STRIPS/PDDL, HTN, heuristic search, beam search, MCTS, and LLM-generated plans.
* **Difficulty:** Principal
* **Category:** Planning
* **The 10-Second Pitch:** Classical planners use explicit states, preconditions, and effects; HTNs decompose tasks; heuristic/beam/MCTS explore alternatives; LLMs propose plans from language. Production agents often combine LLM proposals with symbolic validation and execution feedback.
* **The Deep Dive:** STRIPS represents boolean state and action add/delete lists; PDDL expands types, predicates, numeric/temporal constraints. HTN applies domain methods to recursively decompose goals. A*/heuristic search needs a state model and an admissible or at least informative heuristic; beam bounds frontier width; MCTS estimates action values through selection, expansion, simulation/value, and backup. LLM plans handle open-world language without complete symbolic models but do not guarantee valid preconditions or effects. Compile critical constraints into validators and replan from observed state.

  A production hybrid wraps model-proposed candidates in symbolic precondition checks and validated executors. Compare planners by solution validity, node expansions, model calls, environment interaction cost, and robustness to model error—not theoretical optimality alone.

* **Production Reality & Tradeoffs:** Explicit models are costly to author and become stale; free-form plans are flexible but unreliable. Search cost grows with branching. Choose based on horizon, observability, verifier quality, and action risk.
* **Red Flag:** Calling an ordered natural-language checklist a validated plan.
