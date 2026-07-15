### Q: Explain warmup, cosine/linear/inverse-square-root schedules, restarts, and cooldown.
* **Difficulty:** Senior
* **Category:** Optimization
* **The 10-Second Pitch:** Schedules control optimization phase: warmup avoids unstable early updates, decay reduces late noise, inverse-square-root supports long training, restarts revisit larger steps, and cooldown makes a finite-budget ending explicit.
* **The Deep Dive:** Early Adam moments and representations are poorly estimated; a large target rate can create destructive update-to-weight ratios, especially at large batch. Linear warmup uses $\eta_t=\eta_{max}t/T_w$ (other shapes exist). After warmup, linear decay reaches a floor; cosine uses

$$
\eta_t=\eta_{min}+\tfrac12(\eta_{max}-\eta_{min})\left[1+\cos\left(\pi\frac{t-T_w}{T-T_w}\right)\right].
$$

Inverse-square-root behaves $\eta\propto1/\sqrt t$ after warmup and supports unknown/extended horizons. Restarts reset phase/rate to encourage exploration or anytime training but can disrupt a converging large model. Cooldown sharply or smoothly lowers LR for the final fraction, useful when extending a constant/long schedule.

Specify schedule in optimizer updates or tokens, not ambiguous epochs; gradient accumulation and variable packing change the mapping. Resume must restore step count. Weight decay and momentum integrate over the schedule, so changing horizon without retuning changes regularization.
* **Production Reality & Tradeoffs:** Warmup that is too long wastes budget; too short causes spikes. Cosine is not universally superior to linear. Compare loss versus tokens and wall-clock at equal budget; late low LR can improve loss but offer poor ROI.
* **Red Flag:** Copying “10% warmup” without batch/model stability evidence, or resetting the scheduler accidentally on checkpoint resume.

