### Q: How are images represented as pixels, channels, tensors, patches, regions, and feature maps?
* **Difficulty:** Junior
* **Category:** Conceptual
* **The 10-Second Pitch:** Images enter models as numeric tensors, commonly $[B,C,H,W]$ or $[B,H,W,C]$. Pixels are sampled color values; patches and regions aggregate spatial support; feature maps are learned channel responses retaining some spatial layout.
* **The Deep Dive:** A color image is a discrete sample of a continuous scene, with color space, bit depth, gamma, and normalization affecting values. CNNs produce $[B,C_l,H_l,W_l]$ maps where a location's receptive field grows with depth. ViTs reshape non-overlapping $P\times P\times C$ patches, flatten each to $P^2C$, and linearly project to tokens $[B,N,D]$, where $N=HW/P^2$. Region features may come from detectors or segmentation masks. Coordinate/order information is required because flattening alone loses geometry.
* **Production Reality & Tradeoffs:** Resize/crop policy can remove objects or distort aspect ratio. Channel order, gamma, EXIF rotation, alpha, normalization, and train-serving preprocessing mismatches cause silent regressions. Higher resolution raises tokens and attention cost quadratically.
* **Red Flag:** Calling a patch one pixel or assuming tensor layout is universal.
