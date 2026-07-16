### Q: Explain pooling, anti-aliasing, translation equivariance/invariance, and CNN inductive bias.
* **Difficulty:** Senior
* **Category:** Deep Learning
* **The 10-Second Pitch:** Convolution is translation-equivariant under ideal boundary/sampling conditions; aggregation can create approximate invariance. Strided pooling/convolution downsamples and aliases unless low-pass filtering removes frequencies above the new Nyquist limit.
* **The Deep Dive:** Equivariance means $f(T_\Delta x)=T_\Delta f(x)$; invariance means $f(T_\Delta x)=f(x)$. Shared convolution kernels are equivariant to integer translations away from boundaries. Global average pooling discards spatial location and yields translation-invariant channel summaries, while local max/average pooling only increases local tolerance.

Downsampling by stride $s$ maps multiple high-frequency patterns to the same sampled output. Blur/low-pass before subsampling reduces aliasing and improves shift consistency, but can remove fine detail. Padding breaks exact equivariance near edges; nonlinearities and ordinary stride also cause phase sensitivity. CNN inductive bias is locality, weight sharing, and hierarchical receptive fields, giving sample efficiency for images compared with fully connected models. It is not built-in rotation/scale invariance; augmentations, equivariant architectures, or canonicalization are needed.

```text
high-resolution feature -> low-pass filter -> keep every s-th sample
                           anti-aliasing       downsampling
```
* **Production Reality & Tradeoffs:** Max pooling retains salient peaks but can be unstable; average/blur is smoother. Anti-aliasing costs compute and may hurt tiny-object tasks. Evaluate real camera shifts, crops, borders, and subpixel translations rather than ideal synthetic shifts only.
* **Red Flag:** Saying CNNs are translation-invariant at every layer or that pooling cannot alias because it is not a Fourier transform.

