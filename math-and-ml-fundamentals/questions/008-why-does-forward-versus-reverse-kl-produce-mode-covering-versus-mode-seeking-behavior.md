### Q: Why does forward versus reverse KL produce mode-covering versus mode-seeking behavior?
* **Difficulty:** Senior
* **Category:** Math
* **The 10-Second Pitch:** Forward $D_{KL}(P\|Q)$ averages under $P$ and heavily penalizes $Q\approx0$ wherever $P$ has mass, encouraging coverage. Reverse $D_{KL}(Q\|P)$ averages under $Q$ and heavily penalizes placing $Q$ where $P\approx0$, often selecting one convenient mode.
* **The Deep Dive:** KL is

$$
D_{KL}(P\|Q)=\int p(x)\log\frac{p(x)}{q(x)}\,dx\ge0.
$$

It is asymmetric because the expectation and logarithmic ratio change when arguments swap. Suppose $P$ is a separated two-mode mixture and $Q$ is a single Gaussian. Minimizing $D_{KL}(P\|Q)$ requires $Q$ to assign probability to samples from both modes; otherwise points in a missed mode incur arbitrarily large cost. The resulting Gaussian often spans the low-density valley. Minimizing $D_{KL}(Q\|P)$ evaluates only where $Q$ samples. Centering $Q$ on one mode avoids placing mass in the valley, while ignoring another $P$ mode costs little because $Q$ rarely samples there.

This “covering/seeking” rule is a common outcome, not a theorem for every family. If supports, constraints, or parameterization differ, behavior can change. Forward KL is the objective behind maximum likelihood when the data distribution supplies samples. Standard variational inference commonly minimizes reverse KL because samples/expectations under tractable $Q$ are available.
* **Production Reality & Tradeoffs:** KL is infinite under support mismatch and scale depends on the full distribution. Jensen–Shannon and Wasserstein distances behave differently but introduce their own estimation/gradient issues. In distillation or alignment, direction determines which teacher behaviors are preserved versus which student behaviors are suppressed.
* **Red Flag:** Saying KL is a distance metric, or repeating mode-covering/mode-seeking without identifying which distribution defines the expectation.

