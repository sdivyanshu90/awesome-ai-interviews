### Q: Compare forward-mode, reverse-mode, symbolic, numerical, and implicit differentiation.
* **Difficulty:** Senior
* **Category:** Calculus
* **The 10-Second Pitch:** Forward AD propagates JVPs and scales with input directions; reverse AD propagates VJPs and scales with output seeds. Symbolic differentiates expressions, finite differences approximate derivatives, and implicit differentiation differentiates a solution condition without unrolling the solver.
* **The Deep Dive:** Forward AD augments each primal with tangent $\dot x$ and applies local chain rules to compute $Jv$ in one pass. Reverse AD records/recomputes a graph and propagates adjoints to compute $u^TJ$; scalar-loss training favors reverse mode because one backward pass reaches millions of parameters. Symbolic systems produce derivative expressions that may simplify or explode; AD differentiates the executed program to machine precision, subject to floating arithmetic.

Centered finite difference

$$
\frac{f(x+hv)-f(x-hv)}{2h}=\nabla f(x)^Tv+O(h^2)
$$

is a validation oracle, not a training method: small $h$ increases cancellation. For an implicit solution $F(z^*,\theta)=0$,

$$
\frac{dz^*}{d\theta}=-\left(\frac{\partial F}{\partial z}\right)^{-1}\frac{\partial F}{\partial\theta},
$$

implemented through linear solves/VJPs rather than an inverse. This differentiates optimization layers, equilibrium models, and argmin conditions without storing all iterations.
* **Production Reality & Tradeoffs:** Reverse mode stores activations; checkpointing trades recomputation. Implicit gradients assume a suitable solution/Jacobian and tolerate solver error only approximately. Differentiating control flow follows the executed branch and may be discontinuous.
* **Red Flag:** Calling AD numerical differentiation, or explicitly inverting the implicit Jacobian.

