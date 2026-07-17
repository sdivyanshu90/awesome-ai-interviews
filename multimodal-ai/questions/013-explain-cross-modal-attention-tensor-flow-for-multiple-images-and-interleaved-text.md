### Q: Explain cross-modal attention tensor flow for multiple images and interleaved text.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Cross-modal attention projects language queries against visual keys/values, with media masks restricting which image each text segment can use. Multiple images require explicit token-to-media association and positional identity.
* **The Deep Dive:** Suppose text $X[B,T,D_l]$ and visual features $V[B,M,N,D_v]$. Project/reshape text Q $[B,h,T,d]$, visual K/V after flattening $[B,h,MN,d]$; scores are $[B,h,T,MN]$. A media mask can allow text after image $m$ to attend only that image or all previous images. Gated residual cross-attention returns $[B,T,D_l]$. Alternative early fusion concatenates media tokens under one attention mask.
* **Production Reality & Tradeoffs:** Flattening images without media IDs causes attribution errors. Variable media counts need packing/masks. Cross-attention K/V can be cached, but visual token count drives bandwidth.
With multiple images, assign image and spatial-position identities before projection. A cross-attention block maps text hidden states $Q\in\mathbb R^{B\times T\times D}$ to visual $K,V\in\mathbb R^{B\times N_v\times D}$ and returns $(B,T,D)$; concatenative models instead place $N_v$ visual tokens into the causal stream. Masks must prevent padding/cross-example leakage and preserve which phrase refers to which image.
```text
image_i -> encoder -> projector + image_i/position IDs
text states --Q--> cross-attention <--K,V-- all authorized visual tokens
```

* **Red Flag:** Ignoring which text tokens are allowed to attend which image.
