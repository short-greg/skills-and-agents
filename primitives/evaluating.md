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
2. Assessment produced with evidence — observations documented, verdict based on evidence

## Requirements and Constraints - REQ

1. Evidence must be documented, not just claimed
2. Must output evidence for multiple possible evaluations before deciding (prevents confirmation bias)
3. If insufficient evidence, output what's needed rather than guessing
4. Recommendations are optional — only output when explicitly requested

---

## Protocols

- **criteria_setting.md** — Must use for Establish Criteria (defining evaluation criteria)
- **discipline.md** — Must use for Establish Criteria, Output Evidence Table, Run Automated Tests (systematic enumeration)
- **thinking.md** — Must use for Output Evidence Table, Weight Evidence, Determine Sufficiency (bias prevention, evidence reasoning)
- **software_quality.md** — Use for Execute Manual Tests, Output Evaluation (quality dimensions)
- **system_modularity.md** — Use for Execute Manual Tests (when evaluating design/architecture)
- **pragmatics.md** — Use for Output Evaluation, Provide Recommendations (confidence signaling, framing results)
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

MUST read and follow steps in `base.md`

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

## Possible Actions

Select actions based on context. Each action shows which KR it serves.

### Establish Criteria (→ KR1)

Execute criteria establishment using `criteria_setting.md` (Observable Behavior, Threshold Setting) and `discipline.md` (MECE Enumeration) when evaluation criteria are not clear or not provided. Systematically enumerate ALL criteria that define correct/quality/compliant. Specify observable indicators and thresholds for each criterion. Document what constitutes pass vs fail for each.

### Output Evidence Table (→ KR2)

Execute evidence table creation using `thinking.md` (Analytical) and `discipline.md` (Coverage Tracking) when gathering evaluation evidence. Create table with evidence supporting each possible evaluation systematically: Binary (pass/fail) shows evidence for pass and evidence for fail. Likert (scale) shows evidence for each level. Categorical shows evidence for each category. Rubric shows evidence for each criterion. If evidence is insufficient for any evaluation, document what's missing.

### Run Automated Tests (→ KR2)

Execute automated testing using `discipline.md` (Coverage Tracking) when tests exist and evaluation criteria require testing. Systematically run ALL applicable test types: linting, unit tests, integration tests, end-to-end tests, performance tests, security tests, coverage tests. Document which tests ran, results, and failures with specifics.

### Execute Manual Tests (→ KR2)

Execute manual evaluation using `software_quality.md` (Quality Dimensions) and `system_modularity.md` (Cohesion, Coupling) when automated tests are unavailable or insufficient. For code: read code, check logic, trace execution paths. For design: review against modularity principles. For documents: read for clarity, completeness, accuracy. For claims: check sources, reproduce results, verify logic. Document observations with specific locations and details.

### Weight Evidence (→ KR2)

Execute evidence weighting using `thinking.md` (Analytical, Counterfactual) when deciding final evaluation. Review evidence strength (concrete vs speculative, comprehensive vs partial). Consider evidence quality (reliable sources, reproducible results). Note conflicting evidence. Consider what would change the verdict. Identify strongest case based on evidence weight.

### Determine Sufficiency (→ KR2)

Execute sufficiency check using `thinking.md` (Analytical) when weighing evidence. Check whether judgment can be made with available evidence, whether critical criteria are covered, whether there are major gaps or unknowns. If insufficient: output what's needed. If sufficient: proceed to output evaluation.

### Output Evaluation (→ KR2)

Execute evaluation output using `software_quality.md` (Quality Dimensions) and `pragmatics.md` (Confidence Signaling) when evidence is sufficient. Format depends on evaluation method: Binary outputs pass/fail/inconclusive with supporting evidence. Scale outputs grade with justification. Rubric outputs score per criterion with overall assessment. Signal confidence level appropriately.

### Provide Recommendations (→ KR2)

Execute recommendation generation using `thinking.md` (Strategic) and `pragmatics.md` (Recommended Option) when explicitly requested. Provide concrete next steps to address issues, improvements to strengthen quality, alternative approaches to consider. Frame recommendations clearly with rationale. Recommendations must be actionable, not vague.

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
