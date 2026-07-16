### Q: What mechanisms may explain in-context learning without parameter updates?
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** ICL changes activations and attention state, not weights. The pretrained model may infer the latent task, retrieve analogous patterns, implement learned algorithms such as regression/copying, or update an implicit belief from demonstrations.
* **The Deep Dive:** A prompt supplies observations $(x_i,y_i)$ and a query $x_*$. Forward computation creates hidden states/KV that condition $p(y_*\mid x_*,\{x_i,y_i\})$; after the request, parameters are unchanged. Mechanistic candidates are complementary: pattern completion from training-like sequences; task-vector/latent-variable inference; induction heads that copy continuations; retrieval of semantically similar demonstrations; and learned optimizer/regression-like circuits. Label permutation tests show whether the model follows mappings in context versus pretrained semantics. Demonstration ablation/order/counterfactuals reveal dependence.

```text
weights: fixed
prompt demonstrations -> attention/hidden state -> inferred task/rule -> query prediction
new request -> state disappears unless explicitly persisted
```

ICL differs from fine-tuning, which changes parameters across examples and amortizes behavior into future requests. ICL has limited context/state and pays prompt cost every call, but adapts instantly and keeps variants isolated. It is not guaranteed Bayesian inference or literal gradient descent; those are explanatory models validated only in specific settings.
* **Production Reality & Tradeoffs:** ICL is sensitive to order, formatting, recency, label balance, and context distractions. Demonstrations can leak data or inject instructions. Evaluate held-out task families and counterfactual labels, not examples seen in pretraining.
* **Red Flag:** Saying the model learns new weights during a prompt or that one proposed mechanism explains all ICL.

