### Q: Derive STFT, spectrograms, mel filters, log-mel features, and MFCCs.
* **Difficulty:** Mid
* **Category:** Math
* **The 10-Second Pitch:** STFT windows overlapping waveform frames and Fourier-transforms each; magnitude/power forms a spectrogram. Mel filterbanks aggregate frequencies perceptually, log compresses dynamic range, and a DCT of log-mel energies produces MFCCs.
* **The Deep Dive:** Frame waveform $x[n]$ with window $w[n]$, hop $H$, and FFT size $N$:

$$
X[m,k]=\sum_{n=0}^{N-1}x[n+mH]w[n]e^{-j2\pi kn/N}.
$$

The complex STFT retains magnitude and phase; magnitude/power spectrograms use $|X[m,k]|$ or $|X[m,k]|^2$. Bin spacing is $f_s/N$, and a real signal has $N/2+1$ unique bins. Longer windows narrow frequency bins but smear transients; shorter windows improve temporal localization. Hop controls frame rate and overlap, not the Fourier uncertainty itself.

  A mel bank uses $\operatorname{mel}(f)=2595\log_{10}(1+f/700)$, places overlapping triangular filters uniformly in mel space, and computes $E[m,r]=\sum_k M[r,k]|X[m,k]|^2$. Then $\log(E+\epsilon)$ compresses dynamic range. MFCC applies a DCT, $c_q[m]=\sum_r\log E[m,r]\cos(\pi q(r+1/2)/R)$, retaining low-order coefficients as a compact spectral envelope. Modern neural ASR often keeps the full log-mel grid because MFCC truncation removes detail an encoder can use.
* **Production Reality & Tradeoffs:** Sample rate, resampler, window, hop, FFT size, mel range/count, log floor, dither, and normalization form the model ABI. Streaming must retain window overlap and normalization state; independently padding chunks creates boundary artifacts. Learned waveform front ends may improve with enough data, but add compute and make invariances less explicit.
* **Red Flag:** Calling a spectrogram the raw waveform in image form without windowing/phase considerations.
