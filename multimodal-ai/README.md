# 7. Multimodal AI

Thirty-five interview profiles covering vision encoders, vision-language alignment, audio and speech systems, diffusion models, and production multimodal architectures. Every question has its own answer file.

[Back to the complete syllabus](../SYLLABUS.md)

## Vision and vision-language models

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 001 | [How are images represented as pixels, channels, tensors, patches, regions, and feature maps?](questions/001-how-are-images-represented-as-pixels-channels-tensors-patches-regions-and-feature-maps.md) | Junior | Conceptual |
| 002 | [Derive Vision Transformer patch embeddings, class tokens, positional embeddings, and attention shapes.](questions/002-derive-vision-transformer-patch-embeddings-class-tokens-positional-embeddings-and-attentio.md) | Mid | Math |
| 003 | [Compare CNN, ViT, hierarchical Transformer, and hybrid visual encoders.](questions/003-compare-cnn-vit-hierarchical-transformer-and-hybrid-visual-encoders.md) | Senior | Architecture |
| 004 | [Explain image resizing, cropping, tiling, aspect-ratio preservation, and resolution/token trade-offs.](questions/004-explain-image-resizing-cropping-tiling-aspect-ratio-preservation-and-resolution-token-trad.md) | Senior | Multimodal |
| 005 | [Derive CLIP-style symmetric contrastive learning and temperature scaling.](questions/005-derive-clip-style-symmetric-contrastive-learning-and-temperature-scaling.md) | Senior | Math |
| 006 | [Diagnose representation collapse, shortcut learning, spurious correlations, and cross-dataset shift in vision encoders.](questions/006-diagnose-representation-collapse-shortcut-learning-spurious-correlations-and-cross-dataset.md) | Principal | Debugging |
| 007 | [Compare object detection, segmentation, OCR, captioning, VQA, grounding, and document understanding objectives.](questions/007-compare-object-detection-segmentation-ocr-captioning-vqa-grounding-and-document-understand.md) | Senior | Conceptual |
| 008 | [Compare early fusion, late fusion, cross-attention, co-attention, and shared embedding-space architectures.](questions/008-compare-early-fusion-late-fusion-cross-attention-co-attention-and-shared-embedding-space-a.md) | Senior | Architecture |
| 009 | [How do linear projectors, resamplers, Q-Former, perceiver resampling, and gated cross-attention connect vision to an LLM?](questions/009-how-do-linear-projectors-resamplers-q-former-perceiver-resampling-and-gated-cross-attentio.md) | Senior | Architecture |
| 010 | [Compare frozen-encoder/frozen-LLM training with end-to-end multimodal pretraining.](questions/010-compare-frozen-encoder-frozen-llm-training-with-end-to-end-multimodal-pretraining.md) | Principal | Training |
| 011 | [Design multimodal data mixtures from paired, interleaved, synthetic, OCR, chart, and grounding data.](questions/011-design-multimodal-data-mixtures-from-paired-interleaved-synthetic-ocr-chart-and-grounding.md) | Principal | Data |
| 012 | [How do visual-token count, any-resolution tiling, thumbnail views, and dynamic resolution affect cost and accuracy?](questions/012-how-do-visual-token-count-any-resolution-tiling-thumbnail-views-and-dynamic-resolution-aff.md) | Principal | Systems |
| 013 | [Explain cross-modal attention tensor flow for multiple images and interleaved text.](questions/013-explain-cross-modal-attention-tensor-flow-for-multiple-images-and-interleaved-text.md) | Senior | Math |
| 014 | [Diagnose VLM hallucination, counting, spatial reasoning, OCR, chart, and fine-grained grounding failures.](questions/014-diagnose-vlm-hallucination-counting-spatial-reasoning-ocr-chart-and-fine-grained-grounding.md) | Principal | Debugging |
| 015 | [Evaluate VLMs for answer accuracy, grounding, OCR, robustness, calibration, and safety.](questions/015-evaluate-vlms-for-answer-accuracy-grounding-ocr-robustness-calibration-and-safety.md) | Principal | Evaluation |

## Audio and speech

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 016 | [Explain sampling rate, Nyquist frequency, quantization, waveform amplitude, and aliasing.](questions/016-explain-sampling-rate-nyquist-frequency-quantization-waveform-amplitude-and-aliasing.md) | Junior | Audio |
| 017 | [Derive STFT, spectrograms, mel filters, log-mel features, and MFCCs.](questions/017-derive-stft-spectrograms-mel-filters-log-mel-features-and-mfccs.md) | Mid | Math |
| 018 | [Compare waveform, spectrogram, codec-token, and learned audio representations.](questions/018-compare-waveform-spectrogram-codec-token-and-learned-audio-representations.md) | Senior | Audio |
| 019 | [Compare CTC, RNN-T, encoder-decoder, and decoder-only ASR objectives.](questions/019-compare-ctc-rnn-t-encoder-decoder-and-decoder-only-asr-objectives.md) | Senior | Speech |
| 020 | [Design streaming ASR with VAD, endpointing, chunking, timestamps, punctuation, and partial hypotheses.](questions/020-design-streaming-asr-with-vad-endpointing-chunking-timestamps-punctuation-and-partial-hypo.md) | Principal | System Design |
| 021 | [Explain speaker embeddings, verification, diarization, overlap, and speaker-attribution errors.](questions/021-explain-speaker-embeddings-verification-diarization-overlap-and-speaker-attribution-errors.md) | Senior | Audio |
| 022 | [Compare autoregressive, non-autoregressive, diffusion, and codec-language-model TTS.](questions/022-compare-autoregressive-non-autoregressive-diffusion-and-codec-language-model-tts.md) | Senior | Speech |
| 023 | [Design low-latency voice-to-voice interaction with interruption, echo cancellation, and safety.](questions/023-design-low-latency-voice-to-voice-interaction-with-interruption-echo-cancellation-and-safe.md) | Principal | System Design |
| 024 | [Evaluate ASR/TTS using WER/CER, speaker similarity, intelligibility, naturalness, latency, and fairness.](questions/024-evaluate-asr-tts-using-wer-cer-speaker-similarity-intelligibility-naturalness-latency-and.md) | Principal | Evaluation |

## Diffusion and production multimodal systems

| # | Question | Difficulty | Category |
|---:|---|---|---|
| 025 | [Derive the diffusion forward process, closed-form noising, reverse process, and ELBO connection.](questions/025-derive-the-diffusion-forward-process-closed-form-noising-reverse-process-and-elbo-connecti.md) | Senior | Math |
| 026 | [Compare epsilon, x0, score, and v prediction parameterizations.](questions/026-compare-epsilon-x0-score-and-v-prediction-parameterizations.md) | Senior | Math |
| 027 | [Compare DDPM, DDIM, score-SDE, latent diffusion, flow matching, and rectified flow.](questions/027-compare-ddpm-ddim-score-sde-latent-diffusion-flow-matching-and-rectified-flow.md) | Senior | Generative Models |
| 028 | [Explain U-Net, DiT, VAE latent space, text encoder, timestep embeddings, and cross-attention conditioning.](questions/028-explain-u-net-dit-vae-latent-space-text-encoder-timestep-embeddings-and-cross-attention-co.md) | Senior | Architecture |
| 029 | [Derive classifier guidance and classifier-free guidance, including quality-diversity trade-offs.](questions/029-derive-classifier-guidance-and-classifier-free-guidance-including-quality-diversity-trade.md) | Senior | Math |
| 030 | [Explain schedulers, sampling steps, distillation, consistency models, and fast generation.](questions/030-explain-schedulers-sampling-steps-distillation-consistency-models-and-fast-generation.md) | Senior | Inference |
| 031 | [Design inpainting, image-to-image, ControlNet, reference conditioning, personalization, and editing.](questions/031-design-inpainting-image-to-image-controlnet-reference-conditioning-personalization-and-edi.md) | Principal | System Design |
| 032 | [Diagnose memorization, mode coverage, prompt misalignment, anatomy/text errors, and unsafe generation.](questions/032-diagnose-memorization-mode-coverage-prompt-misalignment-anatomy-text-errors-and-unsafe-gen.md) | Principal | Debugging |
| 033 | [Compare FID, KID, CLIPScore, precision/recall, human preference, and task-specific evaluation.](questions/033-compare-fid-kid-clipscore-precision-recall-human-preference-and-task-specific-evaluation.md) | Principal | Evaluation |
| 034 | [Design a multimodal document-intelligence platform with OCR, layout, tables, VLMs, and review.](questions/034-design-a-multimodal-document-intelligence-platform-with-ocr-layout-tables-vlms-and-review.md) | Principal | System Design |
| 035 | [Design a real-time multimodal assistant combining video frames, audio, memory, tools, and interruption.](questions/035-design-a-real-time-multimodal-assistant-combining-video-frames-audio-memory-tools-and-inte.md) | Principal | System Design |

## Research anchors

- [An Image is Worth 16x16 Words (Vision Transformer)](https://arxiv.org/abs/2010.11929)
- [Learning Transferable Visual Models From Natural Language Supervision (CLIP)](https://arxiv.org/abs/2103.00020)
- [Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2006.11239)
- [High-Resolution Image Synthesis with Latent Diffusion Models](https://arxiv.org/abs/2112.10752)
