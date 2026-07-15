### Q: Explain bias-variance decomposition, double descent, interpolation, and why train/validation gaps can mislead.
* **Difficulty:** Senior
* **Category:** Theory
* **The 10-Second Pitch:** Under squared loss at a fixed input, expected prediction error decomposes into irreducible noise, squared estimator bias, and variance. Double descent shows that capacity near and beyond interpolation can violate the simple U-shaped story; train-validation gaps alone do not identify the cause.
* **The Deep Dive:** Assume $y=f(x)+\epsilon$, $E[\epsilon\mid x]=0$, and train-set-dependent predictor $\hat f_D(x)$. Then

$$
E_{D,\epsilon}\left[(y-\hat f_D(x))^2\right]
=\sigma_\epsilon^2(x)
+\left(E_D[\hat f_D(x)]-f(x)\right)^2
+\operatorname{Var}_D(\hat f_D(x)).
$$

The cross term vanishes by adding/subtracting $E_D\hat f_D$. Bias measures systematic estimator error; variance measures sensitivity to sampled training data. The textbook complexity curve assumes a fixed training procedure and classical regime. Near interpolation, variance can spike; with further overparameterization, implicit bias, optimization, and benign overfitting can reduce test error again—model-wise double descent. Epoch-wise double descent can also occur.

A small train-validation gap can mean both are bad (underfit), leakage/duplicates make validation easy, or strong regularization raises training loss. A large gap can arise from true overfit, augmentation making training harder, label noise, or distribution mismatch. Diagnose with learning curves versus data/compute/capacity, clean and shifted slices, duplicate audits, and multiple seeds.
* **Production Reality & Tradeoffs:** The exact decomposition is loss- and target-dependent and does not include deployment shift. Deep ensembles confound optimization and data variance. Use the framework to design experiments, not to label a model “high bias” from one scalar gap.
* **Red Flag:** Equating every large train-validation gap with excessive model size, or claiming interpolation guarantees good generalization.

