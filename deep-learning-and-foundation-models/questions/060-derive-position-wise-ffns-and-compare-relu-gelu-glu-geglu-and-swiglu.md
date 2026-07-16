### Q: Derive position-wise FFNs and compare ReLU, GELU, GLU, GEGLU, and SwiGLU.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** A Transformer FFN applies the same nonlinear channel transformation independently to each token. ReLU/GELU use one expanded branch; GLU variants multiply a value branch by a learned gate, improving capacity at adjusted width and parameter cost.
* **The Deep Dive:** For $X[B,T,D]$, a basic FFN is

$$
\operatorname{FFN}(X)=\phi(XW_1+b_1)W_2+b_2,
$$

with $W_1[D,F]$, $W_2[F,D]$ and about $2DF$ weights. It mixes features within each token; attention mixes tokens. ReLU is $\max(0,x)$; GELU $x\Phi(x)$ smoothly gates by input magnitude.

GLU forms $(XW_v)\odot\sigma(XW_g)$ then down-projects. GEGLU replaces sigmoid gate with GELU; SwiGLU uses SiLU/Swish: $(XW_v)\odot\operatorname{SiLU}(XW_g)$ followed by $W_d$. It has three matrices ($3DF$), so to match the parameters of a two-matrix FFN with $F=4D$, gated width is often near $8D/3$ (rounding/hardware conventions vary). Multiplication enables input-dependent feature selection and richer interactions.

```text
ReLU/GELU: x -> expand -> activation -> contract
SwiGLU:    x -> value -----* -> contract
             -> gate->SiLU /
```
* **Production Reality & Tradeoffs:** FFNs hold much of model parameters and GEMM FLOPs; width multiples affect tensor-core efficiency and TP sharding. Fused gated kernels reduce intermediate memory. Activation choice interacts with initialization, quantization, and saturation.
* **Red Flag:** Saying FFN processes the sequence dimension or comparing gated versus ungated widths without matching parameters/compute.

