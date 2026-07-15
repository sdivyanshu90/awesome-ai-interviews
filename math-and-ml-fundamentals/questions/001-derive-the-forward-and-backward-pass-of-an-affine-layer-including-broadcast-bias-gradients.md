### Q: Derive the forward and backward pass of an affine layer, including broadcast-bias gradients.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** For $X\in\mathbb{R}^{B\times d_{in}}$, $W\in\mathbb{R}^{d_{in}\times d_{out}}$, and $b\in\mathbb{R}^{d_{out}}$, the layer is $Y=XW+\mathbf{1}b^T$. Given $G=\partial L/\partial Y$, the gradients are $\partial L/\partial X=GW^T$, $\partial L/\partial W=X^TG$, and $\partial L/\partial b=\sum_{i=1}^{B}G_{i,:}$.
* **The Deep Dive:** Write the scalar operation as $Y_{ij}=\sum_{k=1}^{d_{in}}X_{ik}W_{kj}+b_j$. The chain rule gives

$$
\frac{\partial L}{\partial X_{ik}}=\sum_j\frac{\partial L}{\partial Y_{ij}}W_{kj},\qquad
\frac{\partial L}{\partial W_{kj}}=\sum_i X_{ik}\frac{\partial L}{\partial Y_{ij}},\qquad
\frac{\partial L}{\partial b_j}=\sum_i\frac{\partial L}{\partial Y_{ij}}.
$$

The sums reveal why the matrix forms are $GW^T$, $X^TG$, and a reduction over every axis along which $b$ was broadcast. For input shape $[B,T,d_{in}]$ and bias $[d_{out}]$, flatten the leading axes conceptually: $dW=\sum_{b,t}X_{bt:}^TG_{bt:}$ and $db=\sum_{b,t}G_{bt:}$. Do not sum the bias gradient over the feature axis.

A shape proof is a fast interview check: $[B,d_{out}][d_{out},d_{in}]=[B,d_{in}]$ for $dX$, and $[d_{in},B][B,d_{out}]=[d_{in},d_{out}]$ for $dW$. A numerical directional check compares $\langle \nabla_XL,V\rangle$ with $[L(X+\epsilon V)-L(X-\epsilon V)]/(2\epsilon)$.
* **Production Reality & Tradeoffs:** Framework kernels fuse bias and GEMM, accumulate low-precision products in FP32, and may return nondeterministic last-bit differences from parallel reductions. Large effective batches make $dW$ and $db$ reduction-heavy. Custom kernels must handle noncontiguous views, mixed precision, empty batches, and accumulation when a parameter is reused.
* **Red Flag:** Writing $dW=GX^T$, forgetting that a broadcast bias gradient is a sum, or giving formulas without checking shapes.

