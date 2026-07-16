### Q: Why is softmax applied over keys, and what happens with wrong axes or fully masked rows?
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Each query must form a distribution over candidate keys, so scores $[B,H,T_q,T_k]$ normalize along $T_k$. Normalizing another axis changes the operator; an all-masked key row has no valid probability distribution and must be handled explicitly.
* **The Deep Dive:** For query $i$, attention output $o_i=\sum_jp_{ij}v_j$ needs $\sum_jp_{ij}=1$ over keys $j$. Stable masked softmax is

$$
p_{ij}=\frac{A_{ij}e^{s_{ij}-m_i}}{\sum_kA_{ik}e^{s_{ik}-m_i}},\qquad
m_i=\max_{j:A_{ij}=1}s_{ij}.
$$

Softmax over queries would make each key distribute mass among requesters, coupling outputs with different semantics. Softmax over heads makes heads compete; over batch leaks examples. A mask added after softmax leaves forbidden positions in the denominator. Using a finite sentinel such as $-10^4$ may leak under dtype/scale.

If $A_{ij}=0$ for every $j$, both maximum and denominator are undefined; implementations can produce NaN or uniform distribution after finite masking. Avoid creating the row, insert a safe sentinel key then zero the output, or use a masked-softmax kernel explicitly returning zeros. Also mask invalid query outputs/losses.

Test a tiny matrix where expected row sums/weighted values are hand-computed.
* **Production Reality & Tradeoffs:** Fused attention APIs differ in boolean polarity, additive masks, causal offsets, and all-masked behavior. Packed documents, left padding, and cache offsets are common silent failures. Validate gradients too.
* **Red Flag:** Saying softmax axis is arbitrary because the tensor contains the same numbers, or relying on all $-\infty$ rows.

