### Q: Compare DPO, IPO, KTO, ORPO, SimPO, CPO, and online preference optimization.
* **Difficulty:** Principal
* **Category:** Alignment
* **The 10-Second Pitch:** These methods convert preference signals into different offline objectives or reference constraints. DPO uses reference-relative pairwise log odds; alternatives alter loss geometry, reference dependence, unpaired feedback, or combine SFT and preference terms.
* **The Deep Dive:** DPO applies logistic loss to chosen-minus-rejected policy log ratios relative to a reference. IPO uses a squared target/margin motivated by avoiding overfitting separable preferences. KTO can use desirable/undesirable examples without explicit pairs through prospect-theoretic utility. ORPO combines supervised likelihood with an odds-ratio preference penalty without a separate reference. SimPO uses length-normalized policy likelihood and a margin. CPO is Contrastive Preference Optimization (Xu et al. 2024): a reference-free approximation of the DPO loss plus a behavior-cloning term on chosen responses. Online methods collect preferences from current policies, reducing distribution mismatch.
With reference-relative margin $u=\log\frac{\pi_\theta(y_w\mid x)}{\pi_{\mathrm{ref}}(y_w\mid x)}-\log\frac{\pi_\theta(y_l\mid x)}{\pi_{\mathrm{ref}}(y_l\mid x)}$, the pairwise losses differ in geometry:
$$
\mathcal L_{\mathrm{DPO}}=-\log\sigma(\beta u),\qquad
\mathcal L_{\mathrm{IPO}}=\Bigl(u-\tfrac{1}{2\tau}\Bigr)^{2},\qquad
\mathcal L_{\mathrm{SimPO}}=-\log\sigma\!\Bigl(\tfrac{\beta}{|y_w|}\log\pi_\theta(y_w\mid x)-\tfrac{\beta}{|y_l|}\log\pi_\theta(y_l\mid x)-\gamma\Bigr).
$$
DPO's logistic loss saturates, so confidently separated pairs stop contributing; IPO instead regresses $u$ to the fixed target $\tfrac{1}{2\tau}$, keeping gradients bounded on separable preferences; SimPO drops the reference and compares length-normalized policy log likelihoods against margin $\gamma$, directly attacking length inflation. KTO needs no pairs: with implicit reward $r_\theta(x,y)=\beta\log\tfrac{\pi_\theta(y\mid x)}{\pi_{\mathrm{ref}}(y\mid x)}$ and batch-level reference point $z_0\approx D_{\mathrm{KL}}(\pi_\theta\Vert\pi_{\mathrm{ref}})$, it maximizes the prospect-theoretic value $\lambda_D\,\sigma(r_\theta-z_0)$ on desirable examples and $\lambda_U\,\sigma(z_0-r_\theta)$ on undesirable ones, weighting gains and losses around the reference point asymmetrically. Iterative/online DPO regenerates pairs from the current policy each round, scored by a judge or reward model, so the loss stays on-support. Purely off-policy DPO degrades as $\pi_\theta$ drifts from the pair-generating policy: gradients keep shifting mass between two responses the policy no longer produces, while behavior off the pair support is unconstrained.

* **Production Reality & Tradeoffs:** Names/implementations evolve; compare equations, masking, length normalization, beta/margin, and reference. Offline simplicity trades exploration and can overfit biased pairs.
* **Red Flag:** Listing acronyms without deriving how chosen and rejected probabilities are changed.

