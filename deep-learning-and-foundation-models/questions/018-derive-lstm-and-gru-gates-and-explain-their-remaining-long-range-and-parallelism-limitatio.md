### Q: Derive LSTM and GRU gates and explain their remaining long-range and parallelism limitations.
* **Difficulty:** Senior
* **Category:** Deep Learning
* **The 10-Second Pitch:** LSTM creates an additive cell-state path controlled by input/forget/output gates; GRU merges cell/hidden state with update/reset gates. Gates mitigate repeated nonlinear Jacobian shrinkage, but do not guarantee infinite memory and recurrence remains sequential.
* **The Deep Dive:** For $u_t=[x_t;h_{t-1}]$, LSTM computes

$$
i_t=\sigma(W_iu_t),\ f_t=\sigma(W_fu_t),\ o_t=\sigma(W_ou_t),\ \tilde c_t=\tanh(W_cu_t),
$$
$$
c_t=f_t\odot c_{t-1}+i_t\odot\tilde c_t,\qquad h_t=o_t\odot\tanh(c_t).
$$

Ignoring gate dependencies, $\partial c_t/\partial c_{t-1}=f_t$, an additive controlled path; forget bias near positive values encourages early retention. But products of $f_t<1$, saturation, capacity, and interference still limit memory.

A common GRU is $z_t=\sigma(W_z u_t)$, $r_t=\sigma(W_r u_t)$, $\tilde h_t=\tanh(W_xx_t+W_h(r_t\odot h_{t-1}))$, and $h_t=(1-z_t)\odot h_{t-1}+z_t\odot\tilde h_t$ (conventions swap coefficients). GRU is smaller; LSTM exposes a separate cell/output gate. Both share parameters across time and use BPTT.
* **Production Reality & Tradeoffs:** Training/inference require sequential recurrence, limiting GPU parallelism and long-range latency. Truncated BPTT biases dependencies; hidden-state drift and streaming resets matter. Transformers/SSMs trade memory/computation differently but RNNs remain useful at low latency/state.
* **Red Flag:** Saying LSTMs eliminate vanishing gradients or writing gate equations without the additive state path.

