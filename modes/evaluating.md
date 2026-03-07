---
name: evaluating
description: >
  Assessing whether a solution, approach, or claim is correct and/or of quality. Compares target against criteria using evidence-based evaluation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
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

## Steps

MUST read and follow steps in `base_mode.md`

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

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Establish Criteria (→ KR1)

**Goal:** Define what constitutes correct/quality/compliant

**When:** Evaluation criteria are not clear or not provided

**Protocols:** `protocols/criteria_setting.md`, `protocols/discipline.md`, `protocols/risk_management.md`

**Instructions:** Systematically enumerate ALL criteria that define correct/quality/compliant including risk-related criteria. Specify observable indicators and thresholds for each criterion. Document what constitutes pass vs fail for each. Assess risks in the target being evaluated. Use MECE enumeration to ensure completeness and risk assessment techniques.

**Inputs:**
- Evaluation target (required)
- Context or standards (optional)
- Risk tolerance levels (optional)

**Default Output:** Documented evaluation criteria with pass/fail thresholds and risk considerations

### Output Evidence (→ KR2)

**Goal:** Gather evidence for all possible evaluation outcomes

**When:** Gathering evaluation evidence

**Protocols:** `protocols/thinking.md`, `protocols/discipline.md`, `protocols/transparency.md`

**Instructions:** Document evidence supporting each possible evaluation systematically with reasoning. Table format is recommended but can be overridden by evaluation method. Binary (pass/fail) shows evidence for pass and evidence for fail. Likert (scale) shows evidence for each level. Categorical shows evidence for each category. Rubric shows evidence for each criterion. If evidence is insufficient for any evaluation, document what's missing. Use analytical thinking, coverage tracking, and clear documentation of reasoning.

**Inputs:**
- Evaluation criteria (required)
- Evaluation target (required)
- Output format preference (optional)

**Default Output:** Evidence documented (typically as table) showing supporting evidence for each possible evaluation with reasoning

---

### Run Automated Tests (→ KR2)

**Goal:** Execute all applicable automated tests

**When:** Tests exist and evaluation criteria require testing

**Protocol:** `protocols/discipline.md`

**Instructions:** Systematically run ALL applicable test types: linting, unit tests, integration tests, end-to-end tests, performance tests, security tests, coverage tests. Document which tests ran, results, and failures with specifics. Track coverage systematically.

**Inputs:**
- Test suite (required)
- Evaluation target (required)

**Default Output:** Test execution results with pass/fail status and failure details

---

### Execute Manual Tests (→ KR2)

**Goal:** Manually evaluate when automated tests unavailable

**When:** Automated tests are unavailable or insufficient

**Protocols:** `protocols/software_quality.md`, `protocols/system_modularity.md`

**Instructions:** For code: read code, check logic, trace execution paths. For design: review against modularity principles. For documents: read for clarity, completeness, accuracy. For claims: check sources, reproduce results, verify logic. Document observations with specific locations and details. Apply quality dimensions and modularity principles.

**Inputs:**
- Evaluation target (required)
- Evaluation criteria (required)
- Context (optional)

**Default Output:** Manual evaluation findings with specific observations

### Weight Evidence (→ KR2)

**Goal:** Assess evidence strength to determine verdict

**When:** Deciding final evaluation

**Protocol:** `protocols/thinking.md`

**Instructions:** Review evidence strength (concrete vs speculative, comprehensive vs partial). Consider evidence quality (reliable sources, reproducible results). Note conflicting evidence. Consider what would change the verdict. Identify strongest case based on evidence weight. Use analytical and counterfactual thinking.

**Inputs:**
- Evidence table (required)
- Evaluation criteria (required)

**Default Output:** Evidence assessment showing relative strengths

---

### Determine Sufficiency (→ KR2)

**Goal:** Decide if enough evidence exists for judgment

**When:** Weighing evidence

**Protocol:** `protocols/thinking.md`

**Instructions:** Check whether judgment can be made with available evidence, whether critical criteria are covered, whether there are major gaps or unknowns. If insufficient: output what's needed. If sufficient: proceed to output evaluation. Use analytical thinking.

**Inputs:**
- Evidence table (required)
- Evaluation criteria (required)

**Default Output:** Sufficiency determination with gaps identified if insufficient

---

### Output Evaluation (→ KR2)

**Goal:** Produce final evaluation based on evidence

**When:** Evidence is sufficient

**Protocols:** `protocols/software_quality.md`, `protocols/pragmatics.md`

**Instructions:** Format depends on evaluation method: Binary outputs pass/fail/inconclusive with supporting evidence. Scale outputs grade with justification. Rubric outputs score per criterion with overall assessment. Signal confidence level appropriately. Apply quality dimensions and pragmatic framing.

**Inputs:**
- Evidence table (required)
- Evaluation criteria (required)
- Evaluation method (required)

**Default Output:** Final evaluation with supporting evidence and confidence level

---

### Provide Recommendations (→ KR2)

**Goal:** Suggest actionable next steps

**When:** Explicitly requested

**Protocols:** `protocols/thinking.md`, `protocols/pragmatics.md`

**Instructions:** Provide concrete next steps to address issues, improvements to strengthen quality, alternative approaches to consider. Frame recommendations clearly with rationale. Recommendations must be actionable, not vague. Use strategic thinking and pragmatic framing.

**Inputs:**
- Evaluation results (required)
- Context (optional)

**Default Output:** Prioritized, actionable recommendations with rationale

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
