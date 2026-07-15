### Q: How would you gradient-check a custom CUDA/autograd operator without being fooled by numerical error?
* **Difficulty:** Principal
* **Category:** Coding
* **The 10-Second Pitch:** Use tiny smooth float64 cases, centered directional differences over a sweep of step sizes, and compare scale-aware error; then separately test broadcasting, strides, reductions, mixed precision, nondifferentiable points, and accumulation.
* **The Deep Dive:** For random direction $v$, compare analytic $g^Tv$ with

$$
d_h=\frac{L(x+hv)-L(x-hv)}{2h}.
$$

Centered differences have $O(h^2)$ truncation, while rounding error grows as $h$ shrinks; sweep logarithmic $h$ and look for a plateau rather than choosing one epsilon. Use relative error $|d_h-g^Tv|/\max(1,|d_h|,|g^Tv|)$ plus absolute error near zero. Float64 CPU/reference kernels and deterministic reductions isolate algebra before low-precision behavior. Avoid ReLU ties, max ties, clipping boundaries, and masked discontinuities unless testing the documented subgradient.

Check each input/parameter, multiple random directions, full finite-difference coordinates on very tiny tensors, and second-order/double-backward if supported. Cases include noncontiguous/transposed tensors, singleton/broadcast axes, empty dimensions, repeated inputs, aliasing, large/small values, partial/full masks, and loss reductions. Test gradient accumulation by using the operand twice. For CUDA, compare unfused reference, run sanitizers, and test odd shapes/tail blocks.
* **Production Reality & Tradeoffs:** Finite differences validate derivatives of the implemented forward, not whether the forward implements the intended math. Fast-math and atomics change tolerances; mixed-precision kernels need dtype-specific forward/backward accuracy tests after FP64 correctness.
* **Red Flag:** Using only float32 with $h=10^{-8}$, checking at nondifferentiable points, or comparing only one tensor element.

