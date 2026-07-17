### Q: Explain U-Net, DiT, VAE latent space, text encoder, timestep embeddings, and cross-attention conditioning.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** The denoiser is a time-conditioned network: U-Net uses multiscale convolution/skip paths; DiT uses patch Transformers. A VAE compresses images; a text encoder supplies conditioning through cross-attention; timestep embeddings identify noise level.
* **The Deep Dive:** Latent diffusion encodes image $x$ to $z$, noises $z_t$, predicts a target, then VAE-decodes. U-Net downsamples for context and upsamples with skips for detail. DiT patchifies latents and uses attention/MLPs, often adaptive normalization conditioned on time/text. Text tokens become K/V while latent features query them. Timestep sinusoidal/learned embeddings modulate residual blocks. Classifier-free training sometimes drops text to learn unconditional prediction.
* **Production Reality & Tradeoffs:** VAE compression reduces cost but loses small text/detail. Cross-attention can misbind attributes. DiT scales well but attention cost grows with latent tokens. Components are version-coupled.
The VAE compresses pixels and bounds maximum fidelity; U-Net uses multiscale convolutional skip connections, while DiT tokenizes latents and applies transformer blocks. A timestep embedding modulates blocks with noise level. Text encoders produce conditioning tokens consumed through cross-attention or adaptive normalization.
```text
text -> encoder -> conditioning K,V
image -> VAE latent + noise_t -> U-Net/DiT(t, conditioning) -> denoised latent -> VAE decode
```
Freezing text/VAE modules reduces cost but fixes their semantic and reconstruction limitations.

* **Red Flag:** Claiming the text encoder directly paints pixels.
