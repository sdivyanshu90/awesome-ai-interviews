### Q: Compare k-NN, SVM kernels, margin maximization, kernel approximation, and scaling limits.
* **Difficulty:** Senior
* **Category:** Classical ML
* **The 10-Second Pitch:** k-NN stores data and predicts locally under a metric; SVM learns a maximum-margin boundary, with kernels representing nonlinear feature-space inner products. Both depend strongly on scaling and degrade with dimension/size unless approximated.
* **The Deep Dive:** k-NN predicts from the $k$ nearest points, trading low bias at small $k$ for lower variance at larger $k$. Distance concentration, irrelevant dimensions, density variation, and class imbalance break naive Euclidean voting; standardize/learn metrics and use distance-weighting. Training is cheap but exact inference/storage are $O(nd)$ per query; ANN improves speed with recall trade-offs.

For binary SVM, primal soft-margin optimization is

$$
\min_{w,b,\xi}\frac12\|w\|^2+C\sum_i\xi_i
\quad\text{s.t.}\quad y_i(w^T\phi(x_i)+b)\ge1-\xi_i.
$$

Maximizing margin controls norm while slack handles violations. The kernel trick uses $K(x,x')=\langle\phi(x),\phi(x')\rangle$ in the dual; prediction depends on support vectors. RBF width and $C$ jointly control smoothness. Kernel matrices cost $O(n^2)$ memory and superlinear training; random Fourier features/Nyström approximate kernels and return a finite linear problem.
* **Production Reality & Tradeoffs:** Neither raw margin nor neighbor vote is calibrated; use held-out calibration. SVMs handle high-dimensional sparse data well but large nonlinear datasets poorly. k-NN exposes training examples and deletion/privacy costs.
* **Red Flag:** Saying kernels avoid feature engineering at no computational cost, or using unscaled mixed-unit features with Euclidean k-NN.

