### Q: Derive convolution output shapes, parameter counts, receptive field, stride, padding, and dilation.
* **Difficulty:** Mid
* **Category:** Deep Learning
* **The 10-Second Pitch:** For one spatial axis, effective kernel is $d(k-1)+1$ and output size is $\lfloor(n+2p-d(k-1)-1)/s\rfloor+1$. Standard 2D convolution parameters are $C_{out}(C_{in}k_hk_w+1)$ with bias.
* **The Deep Dive:** A cross-correlation-style convolution used by DL computes each output from a local weighted input neighborhood; frameworks usually call it convolution without flipping kernels. For height,

$$
H_{out}=\left\lfloor\frac{H+2p_h-d_h(k_h-1)-1}{s_h}\right\rfloor+1,
$$

and similarly width. Weights have $[C_{out},C_{in}/g,k_h,k_w]$ for groups $g$, so parameters are $C_{out}(C_{in}/g)k_hk_w$ plus optional $C_{out}$ bias.

Receptive-field recursion tracks jump $j_l$ and size $r_l$: $j_l=j_{l-1}s_l$, $r_l=r_{l-1}+(k_l-1)d_lj_{l-1}$. Padding controls border alignment; “same” with stride greater than one may be asymmetric/framework-specific. Stride downsamples and can alias high frequencies; dilation spaces taps without adding weights. Theoretical receptive field exceeds the effective influence distribution, which often concentrates near the center.

```text
k=3,dilation=1: x x x
k=3,dilation=2: x . x . x   effective width 5
```
* **Production Reality & Tradeoffs:** Im2col can expand memory; direct/Winograd/FFT kernels win in different regimes. Layout, groups, odd shapes, and padding drive kernel performance. Add anti-alias filtering before aggressive downsampling when shift robustness matters.
* **Red Flag:** Using $k$ instead of effective dilated kernel in the output formula, or counting parameters proportional to image size.

