### Q: Derive classifier guidance and classifier-free guidance, including quality-diversity trade-offs.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Classifier guidance adds the gradient of conditional log probability to the score. Classifier-free guidance combines conditional and unconditional denoiser predictions, amplifying conditioning at the cost of diversity and possible saturation.
* **The Deep Dive:** Bayes gives $\nabla_x\log p(x\mid c)=\nabla_x\log p(x)+\nabla_x\log p(c\mid x)$. Classifier guidance supplies the second term with a noise-aware classifier. Classifier-free guidance trains with condition dropout and combines predictions as $\epsilon_{guided}=\epsilon_{uncond}+s(\epsilon_{cond}-\epsilon_{uncond})$ under one common convention. Here $s=1$ recovers the conditional prediction; larger $s$ pushes toward condition-correlated directions but reduces diversity and can oversaturate. Guidance rescaling or dynamic thresholding controls some, not all, off-manifold behavior.
* **Production Reality & Tradeoffs:** High CFG can reduce mode coverage, distort anatomy/colors, and amplify prompt artifacts. Negative prompts alter the “unconditional” branch and are not hard constraints. Tune by task and sampler.
Classifier guidance modifies the score as
$$
s_{guided}(x_t)=s_{uncond}(x_t)+w\nabla_{x_t}\log p(y\mid x_t).
$$
Classifier-free guidance learns conditional and dropped-condition predictions and combines them, commonly $epsilon_u+w(\epsilon_c-\epsilon_u)$. Larger $w$ increases prompt adherence but can reduce diversity, oversaturate, and push samples off the learned manifold. The parameterization and scheduler determine equivalent scaling. Tune guidance jointly with steps and negative conditioning, then evaluate diversity as well as preference.

* **Red Flag:** Saying guidance changes model weights during inference.
