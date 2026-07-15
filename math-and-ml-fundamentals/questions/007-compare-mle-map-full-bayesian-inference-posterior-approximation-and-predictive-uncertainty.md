### Q: Compare MLE, MAP, full Bayesian inference, posterior approximation, and predictive uncertainty.
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** MLE returns the likelihood maximizer; MAP returns the posterior mode and is regularized by the negative log prior; full Bayes integrates over the posterior for prediction. Approximate inference trades bias, variance, and compute when that posterior is intractable.
* **The Deep Dive:** Given data $D$ and parameters $\theta$,

$$
\hat\theta_{MLE}=\arg\max_\theta p(D\mid\theta),\qquad
\hat\theta_{MAP}=\arg\max_\theta p(D\mid\theta)p(\theta).
$$

Taking negative logs turns independent examples into a sum. A Gaussian prior $\theta\sim\mathcal N(0,\tau^2I)$ adds $\|\theta\|_2^2/(2\tau^2)$; a Laplace prior adds an L1 term. MAP is still a point estimate and is not invariant to nonlinear reparameterization. Full Bayesian prediction integrates parameter uncertainty:

$$
p(y_*\mid x_*,D)=\int p(y_*\mid x_*,\theta)p(\theta\mid D)\,d\theta.
$$

The posterior denominator $p(D)=\int p(D\mid\theta)p(\theta)d\theta$ is usually intractable. MCMC targets asymptotically exact samples but can mix slowly; variational inference optimizes a tractable $q_\phi(\theta)$ and typically underrepresents some uncertainty; Laplace approximates locally around a mode; ensembles approximate multiple learned hypotheses but are not literal posterior samples. Predictive uncertainty combines conditional data noise with posterior/model uncertainty.
* **Production Reality & Tradeoffs:** Deep-network priors are hard to specify, posteriors are multimodal and symmetry-rich, and approximate uncertainty can be confidently wrong under shift. Compare calibration, selective risk, OOD behavior, and decision utility—not posterior vocabulary. MLE with good regularization/ensembles may outperform a poorly approximated Bayesian model.
* **Red Flag:** Calling MAP “Bayesian uncertainty,” equating dropout with an exact posterior, or predicting only at the posterior mean.

