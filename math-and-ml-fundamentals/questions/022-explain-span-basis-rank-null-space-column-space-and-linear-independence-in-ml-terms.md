### Q: Explain span, basis, rank, null space, column space, and linear independence in ML terms.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** A matrix maps an input space into its column space; rank counts preserved independent directions, while the null space contains indistinguishable input directions. A basis is a minimal independent coordinate system for a span.
* **The Deep Dive:** For $A\in\mathbb R^{m\times n}$, the column space is $\mathcal C(A)=\{Ax:x\in\mathbb R^n\}$ and the null space is $\mathcal N(A)=\{x:Ax=0\}$. The columns are linearly independent when $Ac=0$ implies $c=0$. Rank is $\dim\mathcal C(A)$, and rank–nullity gives $\operatorname{rank}(A)+\dim\mathcal N(A)=n$.

SVD $A=U\Sigma V^T$ makes the geometry explicit: right singular vectors with nonzero singular values span directions preserved by the map; zero-singular-value vectors span the null space; corresponding left singular vectors span the column space. In regression, if feature columns are dependent, a null vector $v$ satisfies $Xv=0$, so $w$ and $w+v$ make identical predictions and coefficients are unidentifiable. In embeddings, numerical rank below width means representations occupy fewer effective directions. Low-rank adapters constrain updates to a small column/row subspace. A basis change changes coordinates but not the underlying span.

```text
input space = row-space directions + null-space directions
                    | A preserves          | A destroys
                    v                      v
               column space               0
```
* **Production Reality & Tradeoffs:** Numerical rank needs a tolerance tied to scale and precision; nearly dependent directions are not exactly null but can be unstable. Regularization selects among equivalent/nearly equivalent solutions rather than creating identifiability. Randomized rank estimates can miss small important directions.
* **Red Flag:** Defining rank as matrix width, or saying a null-space direction is small rather than exactly mapped to zero.

