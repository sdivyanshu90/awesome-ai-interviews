### Q: What does a neural network layer learn, and why are depth, width, and nonlinearities necessary?
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** A layer learns a task-optimized change of representation. Width supplies simultaneous features, depth composes transformations and interactions, and nonlinearities prevent the entire stack from collapsing to one affine map.
* **The Deep Dive:** An affine layer $h'=Wh+b$ selects, mixes, and scales directions from its input. Without nonlinearities, $W_2(W_1x+b_1)+b_2$ is just $W'x+b'$, so arbitrary depth adds no function class. An activation, gate, attention softmax, or multiplicative interaction changes that: ReLU networks partition space into linear regions, smooth activations bend it, and gates condition one feature on another.

Width controls the number of features/intermediate channels available at one stage and can approximate broad functions when large enough. Depth expresses hierarchical/compositional computation—edges to textures to objects, or characters to syntax to semantics—often with far fewer units than a shallow representation of the same interaction. Residual connections let layers learn incremental updates and preserve information.

```text
input coordinates -> learned features -> feature interactions -> task representation -> output
      affine + nonlinearity repeated; residual paths retain earlier information
```

Universal approximation is an existence theorem: it does not guarantee finite data, efficient parameters, trainability, robustness, or extrapolation. The loss and data determine what features are useful; neurons need not map to human concepts.
* **Production Reality & Tradeoffs:** More width/depth increase compute, memory, optimization difficulty, and overfitting/data demand. Bottlenecks can discard information; excessive width may be memory-bound. Interpret representations with ablations/probes/interventions, not activation anecdotes.
* **Red Flag:** Saying each neuron learns one concept, or that stacking linear layers creates nonlinear decision boundaries.

