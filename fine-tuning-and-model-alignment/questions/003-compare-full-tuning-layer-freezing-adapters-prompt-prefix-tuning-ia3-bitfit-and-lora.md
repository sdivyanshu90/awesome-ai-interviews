### Q: Compare full tuning, layer freezing, adapters, prompt/prefix tuning, IA3, BitFit, and LoRA.
* **Difficulty:** Senior
* **Category:** Architecture
* **The 10-Second Pitch:** Full tuning updates all weights; parameter-efficient methods freeze the base and learn small task-specific components. LoRA learns low-rank weight updates, QLoRA stores frozen base weights in 4-bit while training adapters, and other methods alter prompts, activations, or scaling vectors.
* **The Deep Dive:** LoRA replaces $W$ effectively with $W+\frac\alpha r BA$, where $A$ and $B$ have rank $r$. It can target Q/K/V/O and MLP projections. Prefix/prompt tuning learns virtual tokens; IA3 learns multiplicative vectors. QLoRA uses quantized frozen weights plus higher-precision adapter computation and optimizer states.
Efficiency has several axes: trainable weights, optimizer and activation memory, checkpoint size, switching cost, and capacity. BitFit changes biases; IA3 scales channels; prefixes inject attention state; adapters add bottlenecks; LoRA bounds update rank. Merging LoRA removes runtime adapter work but sacrifices cheap switching. Compare equal tokens, schedules, targets, precision, and serving conditions. PEFT reduces optimizer state, not necessarily activations or base-model inference memory.

* **Production Reality & Tradeoffs:** PEFT reduces trainable memory and enables many tenants, but not necessarily activation memory or base-model inference cost. Target modules, rank, alpha, data size, and model family need ablation.
* **Red Flag:** Saying LoRA “fine-tunes fewer neurons” or believing 4-bit QLoRA backpropagates updates into quantized base weights.
