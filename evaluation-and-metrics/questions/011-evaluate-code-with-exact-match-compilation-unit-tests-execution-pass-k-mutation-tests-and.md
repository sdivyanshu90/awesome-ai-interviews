### Q: Evaluate code with exact match, compilation, unit tests, execution, pass@k, mutation tests, and security scans.
* **Difficulty:** Senior
* **Category:** Code Evaluation
* **The 10-Second Pitch:** Code is behavior, not text: progressively validate syntax/build, isolated execution, hidden and property tests, mutation resistance, performance, and security; estimate pass@k from independent samples without cherry-picking.
* **The Deep Dive:** Exact match is useful only for canonical transformations; compilation catches syntax/type/link errors but not semantics. Run candidate code in a resource-limited sandbox with deterministic dependencies, no secrets, restricted network/filesystem, time/memory/process limits, and captured artifacts. Tests should include public examples, hidden boundaries, randomized/property checks, concurrency, and performance constraints. Mutation testing reveals weak oracles; static analysis, SBOM scanning, secret detection, and dynamic tests cover unsafe behavior. If $n$ independent samples contain $c$ correct solutions, the unbiased estimator is

$$
\operatorname{pass@}k=1-\frac{\binom{n-c}{k}}{\binom nk},\qquad n-c\ge k.
$$

Do not estimate pass@$k$ from only the selected top-$k$ outputs; the formula assumes a larger sampled pool.
* **Production Reality & Tradeoffs:** Tests are incomplete and can be gamed or contaminated. Sandbox escapes are platform risks. pass@k rises with sampling budget and is not single-attempt user success. Track flaky tests, nondeterminism, runtime, and severity of failures.
* **Red Flag:** Calling code correct because it compiles or reporting pass@k after manually choosing favorable samples.
