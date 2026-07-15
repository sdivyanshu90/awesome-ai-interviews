### Q: Explain partial derivatives, gradients, directional derivatives, Jacobians, and Hessians.
* **Difficulty:** Junior
* **Category:** Calculus
* **The 10-Second Pitch:** Partials vary one coordinate; the gradient represents the local linear map for a scalar; a directional derivative projects it onto a direction; a Jacobian handles vector outputs; a Hessian captures second-order scalar curvature.
* **The Deep Dive:** For $f:\mathbb R^n\to\mathbb R$, the differential is $df\approx\nabla f(x)^Tdx$ and the directional derivative along $v$ is

$$
D_vf(x)=\lim_{\epsilon\to0}\frac{f(x+\epsilon v)-f(x)}{\epsilon}=\nabla f(x)^Tv.
$$

The gradient depends on the chosen inner product/coordinates even though the differential is intrinsic. For $g:\mathbb R^n\to\mathbb R^m$, the output-by-input Jacobian $J_{ij}=\partial g_i/\partial x_j$ gives $g(x+\delta)\approx g(x)+J\delta$. For scalar $f$, $H_{ij}=\partial^2f/(\partial x_i\partial x_j)$ and

$$
f(x+\delta)\approx f(x)+\nabla f^T\delta+\tfrac12\delta^TH\delta.
$$

$v^THv$ is curvature along $v$. Backprop computes VJPs without storing $J$; forward-mode computes JVPs. Example $f(x,y)=x^2y$: $\nabla f=(2xy,x^2)$ and direction $v$ changes $f$ at $2xyv_x+x^2v_y$.
* **Production Reality & Tradeoffs:** Hessians are symmetric only under regularity and can be indefinite in nonconvex losses. At ReLU kinks classical derivatives fail and frameworks choose subgradients. Always state shapes and row/column convention.
* **Red Flag:** Calling the Jacobian a scalar derivative or confusing the gradient vector with the Hessian matrix.

