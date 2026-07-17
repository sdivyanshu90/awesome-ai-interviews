### Q: Explain sampling rate, Nyquist frequency, quantization, waveform amplitude, and aliasing.
* **Difficulty:** Junior
* **Category:** Audio
* **The 10-Second Pitch:** Sampling converts continuous audio to discrete amplitudes. Frequencies above half the sample rate alias into lower frequencies unless filtered; bit depth controls amplitude quantization and dynamic range.
* **The Deep Dive:** At rate $f_s$, unique representable frequencies lie below Nyquist $f_s/2$ for band-limited reconstruction. Real ADCs apply anti-alias filters. Quantization maps amplitudes to finite levels; uniform $b$-bit PCM has about $6.02b$ dB ideal dynamic range. Normalized floating waveform typically lies near $[-1,1]$, but clipping destroys peaks. Resampling requires low-pass filtering before decimation.
* **Production Reality & Tradeoffs:** Speech may tolerate 16 kHz; music/high-frequency tasks need more. Incorrect sample-rate metadata changes pitch/time. Clipping, DC offset, channel mixing, and train-serving codecs cause failures.
Sampling at $f_s$ Hz maps continuous frequencies above $f_s/2$ onto lower aliases; an anti-alias low-pass filter must precede downsampling. Quantization maps amplitude to finite levels, adding signal-dependent error and clipping if range is exceeded. Bit depth controls nominal dynamic range; sample rate controls representable bandwidth, not loudness. Demonstrate aliasing with $f_{alias}=|f-kf_s|$ for an integer $k$ yielding the baseband frequency. Resampling quality depends on filter transition bandwidth and phase behavior.

* **Red Flag:** Saying higher sample rate always improves a model regardless of bandwidth and cost.
