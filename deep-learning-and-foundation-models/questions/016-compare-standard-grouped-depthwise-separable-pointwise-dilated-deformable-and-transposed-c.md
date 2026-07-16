### Q: Compare standard, grouped, depthwise-separable, pointwise, dilated, deformable, and transposed convolutions.
* **Difficulty:** Senior
* **Category:** Deep Learning
* **The 10-Second Pitch:** These variants change channel connectivity, spatial sampling, or upsampling: grouping reduces cross-channel mixing, depthwise plus pointwise factorizes it, dilation expands support, deformable learns offsets, and transposed convolution is the input-gradient linear operator—not a literal inverse.
* **The Deep Dive:** Standard convolution connects every input to every output channel. Grouped convolution partitions channels into $g$ independent sets, reducing parameters/FLOPs by $g$; depthwise uses $g=C_{in}$, one spatial filter per channel. A $1\times1$ pointwise convolution then mixes channels, giving depthwise-separable cost $C_{in}k^2+C_{in}C_{out}$ instead of $C_{in}C_{out}k^2$. Dilated convolution samples spaced positions, expanding receptive field without new weights but can create gridding. Deformable convolution predicts offsets (and sometimes modulation) then samples with interpolation, adapting geometry at extra complexity.

A transposed convolution applies the transpose of the convolution matrix, expanding spatial resolution according to stride/padding/output-padding. Uneven kernel overlap can create checkerboard artifacts; resize-then-convolve is an alternative. None of these is automatically more expressive at fixed compute—channel shuffle or later pointwise mixing may be required after groups.

| Variant | Changes | Typical goal |
|---|---|---|
| grouped/depthwise | channel connectivity | efficiency |
| dilated | sampling spacing | context |
| deformable | learned coordinates | geometry |
| transposed | output lattice | learned upsampling |
* **Production Reality & Tradeoffs:** Depthwise operations can be memory-bound and slower than dense kernels on some hardware. Deformable sampling complicates export/quantization. Test aliasing, borders, and deployment kernel support, not FLOPs alone.
* **Red Flag:** Calling transposed convolution the mathematical inverse of convolution or assuming fewer FLOPs guarantees lower latency.

