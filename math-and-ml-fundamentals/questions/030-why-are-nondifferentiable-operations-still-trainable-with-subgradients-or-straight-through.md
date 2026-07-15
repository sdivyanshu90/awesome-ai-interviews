### Q: Why are nondifferentiable operations still trainable with subgradients or straight-through estimators?
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** Convex kinks admit subgradients that support descent guarantees; discrete operations do not, so straight-through estimators deliberately optimize a surrogate backward path. STEs are biased and succeed only when that mismatch is tolerable.
* **The Deep Dive:** For convex $f$, $g$ is a subgradient at $x$ if $f(y)\ge f(x)+g^T(y-x)$ for all $y$. At zero, $\partial|x|=[-1,1]$; ReLU has a conventional subgradient choice. Subgradient descent uses diminishing steps and converges more slowly than smooth gradient methods.

Rounding, argmax, sampling, and hard routing are constant almost everywhere with undefined jumps, so their true pathwise gradient is zero/undefined. An STE uses a hard forward $y=\operatorname{round}(x)$ but substitutes, for example, $\partial y/\partial x\approx1$ or a clipped identity backward. It is not the derivative of the executed function. Alternatives include smooth relaxations (softmax/Gumbel-softmax), reparameterization for suitable random variables, and score-function estimators

$$
\nabla_\theta E_{x\sim p_\theta}[f(x)]=E[f(x)\nabla_\theta\log p_\theta(x)],
$$

which are unbiased but often high variance. Control variates reduce variance.
* **Production Reality & Tradeoffs:** Annealed relaxations can create train–serve mismatch; STEs can point uphill for the true discrete objective; REINFORCE needs many samples. Evaluate hard-forward behavior, gradient variance, estimator bias, and sensitivity to temperature/clipping.
* **Red Flag:** Claiming the STE is the true derivative of rounding, or using argmax in the forward path and expecting ordinary backprop to work.

