### Q: Diagnose benchmark contamination, memorization, data repetition, and domain imbalance.
* **Difficulty:** Principal
* **Category:** Data
* **The 10-Second Pitch:** Trace benchmarks against every training derivative with exact/fuzzy/semantic/code-aware matching, use canaries and time splits, separate verbatim recall from generalization with counterfactuals, and audit effective token mixture/repetition by domain.
* **The Deep Dive:** Contamination includes exact question/answer, paraphrases, translations, solution discussions, benchmark-derived synthetic data, and chunk overlap. Normalize carefully, hash exact spans, use n-gram/MinHash/edit/code-AST matching, embedding candidates with human verification, and source lineage. Check both prompts and answers; answer-only overlap is highly diagnostic. Temporal benchmarks after the data cutoff and private canaries provide stronger evidence.

Memorization tests prompt prefixes and measures verbatim continuation, exposure, nearest-training similarity, and sensitivity to changed names/numbers. A model that answers only the original but fails a semantics-preserving counterfactual likely memorized. Repetition audit counts effective occurrences after sampling/epochs, not unique files; duplicated high-quality data can improve optimization before overfitting, while repeated rare/PII strings increase extraction risk. Domain imbalance needs token and example counts, fertility, quality, loss/gradient contribution, and capability by language/domain.

```text
source -> derivatives -> dedup groups -> train shards
benchmark -------- overlap at every level --------^
```
* **Production Reality & Tradeoffs:** Pretraining corpora may be unavailable, making proof impossible; disclose uncertainty. Semantic matches have false positives. Removing contaminated items after evaluation does not restore an unbiased tuning process; use fresh confirmation sets.
* **Red Flag:** Checking filenames or exact prompts only, or calling any correct benchmark answer memorization.

