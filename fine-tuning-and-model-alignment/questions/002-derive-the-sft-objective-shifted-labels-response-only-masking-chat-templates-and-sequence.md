### Q: Derive the SFT objective, shifted labels, response-only masking, chat templates, and sequence packing.
* **Difficulty:** Senior
* **Category:** Deep Learning
* **The 10-Second Pitch:** SFT is next-token cross-entropy on serialized demonstrations, usually masking system/user/tool tokens and training loss only on desired assistant tokens. The exact chat template is part of the data distribution and must match inference.
* **The Deep Dive:** A conversation becomes one causal sequence with role delimiters and turn boundaries. Labels at ignored positions are excluded; assistant response tokens predict the next assistant tokens under prior context. Multi-turn data can train every assistant turn or only a final one, which changes the objective. Incorrect EOS handling teaches run-on responses or role leakage.
For tokens $z_1,\ldots,z_T$ and response mask $m_t$,
$$
\mathcal L=-\frac{\sum_{t=1}^{T-1}m_{t+1}\log p_\theta(z_{t+1}\mid z_{\le t})}{\sum_{t=1}^{T-1}m_{t+1}}.
$$
Logits at $t$ predict label $t+1$. Packed samples need separate attention/loss boundaries. Hand-test role delimiters, shifted masks, EOS labels, and the denominator; many apparent optimizer failures are preprocessing bugs.

* **Production Reality & Tradeoffs:** Keep raw examples, rendered tokens, template version, tokenizer version, and split provenance. Measure token-weighted loss and conversation-level quality; long examples can dominate training otherwise.
* **Red Flag:** Training on formatted chat without verifying which tokens contribute to loss.
