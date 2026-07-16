### Q: Derive causal, padding, document-boundary, sliding-window, and block-sparse masks.
* **Difficulty:** Senior
* **Category:** Implementation
* **The 10-Second Pitch:** Every mask defines allowed query–key edges. Combine causal order, valid-token padding, document identity, and locality/sparsity before softmax; shape it broadcastably and explicitly handle queries with no valid key.
* **The Deep Dive:** Let $A_{ij}\in\{0,1\}$ indicate query $i$ may attend key $j$, and additive mask $M_{ij}=0$ if allowed, $-\infty$ otherwise. Causal: $j\le i$ (adjust for cached prefix offsets). Key padding: key $j$ must be valid; invalid query outputs should also be zeroed/ignored. Packed documents require $doc(i)=doc(j)$ in addition to causality, preventing leakage across examples. Sliding window adds $i-W<j\le i$; global tokens create exceptions. Block-sparse masks define allowed block pairs, but boundary blocks still need element masks.

$$
A_{ij}=1[j\le i]\,1[valid(j)]\,1[doc(i)=doc(j)]\,1[i-j<W]
$$

for the intersection example. With scores $[B,H,T_q,T_k]$, masks may originate as $[B,T_k]$, $[T_q,T_k]$, or block metadata; verify broadcasting. In incremental decode, query absolute position differs from local row index. Fully masked rows yield `0/0`/NaN under ordinary softmax; exclude them or use a safe masked softmax returning zero.
* **Production Reality & Tradeoffs:** Materializing dense masks defeats sparse attention. Boolean versus additive mask conventions differ across APIs, and finite large negatives can leak in FP16. Unit-test tiny expected matrices, packed boundaries, left/right padding, cache offsets, and all-masked rows.
* **Red Flag:** Applying separate softmaxes for masks, masking after softmax, or assuming a triangular mask alone makes packed training safe.

