### Q: Distinguish hallucination, uncertainty, memorization, reasoning failure, and retrieval failure mechanistically.
* **Difficulty:** Principal
* **Category:** Foundation Models
* **The 10-Second Pitch:** These failures occur at different stages: generation asserts unsupported content; uncertainty concerns predictive/evidence ambiguity; memorization reproduces training instances; reasoning maps available premises incorrectly; retrieval fails to supply relevant evidence. Diagnose with provenance and controlled interventions.
* **The Deep Dive:** Decompose the system: user/task interpretation, corpus/index/query/filter/ranker, context serialization, model conditional generation, tool calls, and postprocessing. Retrieval failure means support exists in the authorized corpus but is absent/misranked/truncated; oracle-context replacement should recover the answer. Reasoning failure persists when complete correct premises are supplied and a simple verifier shows invalid inference. Hallucination labels an unsupported/contradicted generated claim and can result from either missing evidence or generation ignoring it. Uncertainty is a distribution over plausible outcomes/evidence and is not reliably measured by self-reported confidence. Memorization is unusually specific reproduction from training; counterfactual value swaps and nearest-training analysis distinguish it from learned procedure.

```text
oracle retrieval fixes -> retrieval/context failure
oracle premises present, inference wrong -> reasoning failure
verbatim original succeeds, counterfactual fails -> memorization suspicion
claim lacks/contradicts evidence -> hallucinated output
multiple plausible evidence/outcomes -> uncertainty
```
* **Production Reality & Tradeoffs:** Categories overlap: memorized false text can be hallucinated; retrieval conflict creates uncertainty. Use atomic claims, source/time provenance, stage traces, and severity. Fix the causal layer instead of adding a generic prompt.
* **Red Flag:** Calling every incorrect response a hallucination, which prevents targeted remediation.

