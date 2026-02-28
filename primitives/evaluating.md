---
name: evaluating
description: >
  Assessing whether a solution, approach, or claim is correct and/or of quality. Compares target against criteria using evidence-based evaluation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "verify this", "test this", "does this work", "check if correct", "validate the result", "run the tests", "confirm this", "is this right",
  "critique this", "review this", "what's wrong with this", "how can I improve", "code review", "give me feedback", "find issues", "red team this", "poke holes".
  keywords: validating, verifying, critiquing
argument-hint: "[what to evaluate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Evaluating

**Goal:** Assess target against criteria through systematic evidence-based evaluation.

**Intent:** Determine correctness, quality, or compliance before decisions are made. Prevent shipping broken work, adopting flawed approaches, or accepting false claims.

**Scope:** Comparing an evaluation target against evaluation criteria using systematic methods. Outputs assessment based on evidence. Optionally provides recommendations when requested.

---

## Key Results - KR

1. Evaluation criteria established — what constitutes correct/quality/compliant is defined
2. Evidence collected and assessed — observations, test results, comparisons documented
3. Evaluation output — verdict, grade, or assessment based on evidence

## Requirements and Constraints - REQ

1. Evidence must be documented, not just claimed
2. Must output evidence for multiple possible evaluations before deciding (prevents confirmation bias)
3. If insufficient evidence, output what's needed rather than guessing
4. Recommendations are optional — only output when explicitly requested

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to find evidence for multiple possible evaluations before deciding
- **tracking.md** - Must use to track evaluation steps systematically
- **quality.md** - Use when assigning severity or prioritizing issues
- **modularity.md** - Use when evaluating design or architecture
- **recovery.md** - Use when resuming after interruption

---

## Terms

**Evaluation Target:** What is being evaluated (code, design, solution, hypothesis, claim, result, document, artifact, process)

**Evaluation Method:** How to evaluate the target — the criteria, methodology, or standards to apply (binary pass/fail, Likert scale, rubric, checklist, comparison to reference, etc.)

---

## Preconditions

**Required:** Evaluation target and evaluation method

**Elicit if not provided:**
- What to evaluate (evaluation target)
- How to evaluate it (evaluation method, criteria, standards)

**Optional:**
- Whether recommendations are requested
- Context (project conventions, constraints, prior evaluations)
- Reference materials (specs, baselines, examples)

## Postconditions

**Success:** Evaluation output according to evaluation method and target

**Success (optional):** Recommendations output if explicitly requested

**Failure:** Cannot access target, criteria undefined, evaluation method unavailable, insufficient evidence to evaluate

---

## Actions

Select actions based on context. Each action shows which KR it serves.

### Output Evidence Table (→ KR2, KR3)

Execute evidence table creation using reasoning.md to prevent confirmation bias when gathering evaluation evidence. Create table with evidence supporting each possible evaluation: Binary (pass/fail) shows evidence for pass and evidence for fail. Likert (scale) shows evidence for each level (1-5, poor-excellent, etc.). Categorical shows evidence for each category (low/medium/high risk, A/B/C grade, etc.). Rubric shows evidence for each criterion. If evidence is insufficient for any evaluation, document what's missing.

### Run Automated Tests (→ KR2)

Execute automated testing using Bash to gather objective evidence when tests exist and evaluation criteria require testing. Run linting for style/formatting/conventions, unit tests for function correctness/edge cases, integration tests for component interactions, end-to-end tests for full workflow validation, performance tests for speed/resource usage, security tests for vulnerability scanning, coverage tests for code coverage metrics. Document which tests ran, results, and failures.

### Execute Manual Tests (→ KR2)

Execute manual evaluation using Read and Grep to gather evidence when automated tests are unavailable or insufficient. For code: read code, check logic, trace execution paths. For UI: interact with interface, verify behavior, check accessibility. For design: review against principles (SOLID, coupling/cohesion, modularity.md). For documents: read for clarity, completeness, accuracy. For claims: check sources, reproduce results, verify logic. For processes: observe execution, check adherence to standards. Document observations with specific locations and details.

### Weight Evidence (→ KR3)

Execute evidence weighting using reasoning.md to compare strength across possible evaluations when deciding final evaluation. Review evidence strength (concrete vs speculative, comprehensive vs partial). Consider evidence quality (reliable sources, reproducible results). Note conflicting evidence. Identify strongest case based on evidence weight.

### Determine Sufficiency (→ KR3)

Execute sufficiency check using reasoning.md to validate completeness when weighing evidence. Check whether judgment can be made with available evidence, whether critical criteria are covered, whether there are major gaps or unknowns. If insufficient: output what's needed (more investigation, clearer criteria, additional context, etc.). If sufficient: proceed to output evaluation.

### Output Evaluation (→ KR3)

Execute evaluation output using quality.md to format results when evidence is sufficient. Format depends on evaluation method: Binary outputs pass/fail/inconclusive with supporting evidence. Scale outputs numeric or qualitative grade with justification. Categorical outputs category assignment with reasoning. Rubric outputs score per criterion with overall assessment. Include final evaluation, evidence summary, specific issues/strengths with locations, severity (if applicable).

### Provide Recommendations (→ KR3)

Execute recommendation generation using reasoning.md to suggest next steps when explicitly requested. Provide concrete next steps to address issues, improvements to strengthen quality, alternative approaches to consider, what to investigate further. Recommendations must be actionable, not vague.

---

## Additional Notes and Terms

**Evaluation vs Investigation:** Evaluation compares against criteria using available evidence. Investigation gathers new evidence through research. If evaluation reveals insufficient evidence, investigation may be needed.

**Evaluation types:** Binary (correct/incorrect), Scale (poor to excellent), Categorical (levels), Rubric (multiple criteria)

---

## References

**Verification:**
- [Software Testing Fundamentals](https://softwaretestingfundamentals.com/)
- [IEEE Standard for Software Verification and Validation](https://standards.ieee.org/ieee/1012/5609/)

**Quality Assessment:**
- [Google Engineering Practices - Code Review](https://google.github.io/eng-practices/review/)
- [Google - What to Look For](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [Nielsen Norman Group - Heuristic Evaluation](https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/)
- [Nielsen's 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)
- [Better Evaluation - Evaluation Framework](https://www.betterevaluation.org/methods-approaches/methods/evaluation-framework)
- [NCBI - Critical Appraisal](https://www.ncbi.nlm.nih.gov/books/NBK603121/)
