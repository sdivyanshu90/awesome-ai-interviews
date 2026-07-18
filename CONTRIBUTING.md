# Content authoring contract

Every interview profile is a standalone Markdown file under `<domain>/questions/` and is linked from that domain's `README.md`.

## Required profile structure

Use each field exactly once and in this order:

```markdown
### Q: Specific interview question
* **Difficulty:** Junior / Mid / Senior / Principal
* **Category:** Category
* **The 10-Second Pitch:** Precise answer.
* **The Deep Dive:** Mechanics, derivation, tensor/data flow, and verification.
* **Production Reality & Tradeoffs:** Failure modes, cost, latency, scale, and operations.
* **Red Flag:** A concrete misconception or omitted invariant.
```

The deep dive must answer the actual question. Do not pad profiles with generic interview advice. State assumptions, dimensions, probability conventions, state transitions, and evaluation criteria where applicable. Include a limiting case, counterexample, failure trace, or test that could falsify the claim.

## GitHub-rendered mathematics

Use GitHub's supported MathJax delimiters:

- Inline: `$p(y\mid x)$`
- Display:

  ```markdown
  $$
  \operatorname{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}+M\right)V
  $$
  ```

Do not use backslash-parenthesis or backslash-bracket LaTeX delimiters; GitHub does not treat those consistently as repository Markdown math. Do not put mathematical derivations in inline-code backticks. Backticks are reserved for identifiers, literal values, commands, schemas, and code.

Define symbols before using them, state tensor shapes and reduction axes, and distinguish proportionalities/approximations from equalities. Large derivations should use display math and a following sentence explaining the operational meaning.

## Visuals

Use a visual when it clarifies a nontrivial data flow, state transition, hierarchy, tensor transformation, or failure path. Prefer repository-native diagrams:

- `mermaid` for architecture, flows, and state machines;
- `text` for compact tensor/control-path diagrams;
- code blocks for algorithms where execution order is the point.

A visual must be explained in prose and must not replace authorization, failure, or consistency details. Each domain maintains at least three question profiles with substantive visuals or executable pseudocode.

## Quality gate

Run:

```bash
./scripts/validate-content.sh
```

The gate checks profile count and structure, minimum explanatory depth, unsupported math delimiters, balanced fences, README mappings, boilerplate regressions, and domain visual coverage.
