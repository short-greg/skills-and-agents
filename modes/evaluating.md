---
name: evaluating
description: >
  Assessing whether a solution, approach, or claim is correct and/or of quality. Compares target against criteria using evidence-based evaluation.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "verify this", "test this", "does this work", "check if correct", "validate the result",
  "critique this", "review this", "what's wrong with this", "code review", "give me feedback", "red team this".
  keywords: selecting, applying, evaluating, collecting, assessing, reporting, recommending
---

# Evaluating

**Goal:** Assess target against criteria through systematic evidence-based evaluation.

**Intent:** Determine correctness, quality, or compliance before decisions are made. Prevent shipping broken work, adopting flawed approaches, or accepting false claims.

**Scope:** Comparing an evaluation target against evaluation criteria using systematic methods. Answers "is this correct/quality/compliant?" not "what should we build?" (defining) or "what do we need to know?" (investigating).

---

## Key Results - KR

1. Evaluation criteria selected — applicable criteria identified from standards, specs, conventions, or user-provided
2. Assessment produced with evidence — evidence gathered for multiple possibilities, verdict based on weighted evidence

---

## Preconditions

**Required:** Evaluation target (what to evaluate), evaluation criteria or source of criteria (standards, specs, conventions)

**Optional:**
- Whether recommendations are requested
- Context (project conventions, constraints, prior evaluations)
- Reference materials (specs, baselines, examples)

## Postconditions

**Success:** Evaluation output with evidence-based verdict according to selected criteria

**Failure:** Cannot access target, no applicable criteria found, insufficient evidence to evaluate. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Select Criteria | KR1 | To identify which criteria apply to this evaluation | Use when starting evaluation or criteria not yet identified | Read and apply appropriate techniques from `protocols/criteria_setting.md` to identify applicable criteria from standards, specs, conventions, or user requirements | In: Evaluation target (required), Available standards or specs (optional) | Out: Applicable criteria with pass/fail thresholds |
| Apply Evaluation Methods | KR2 | To execute programmatic evaluation and gather evidence | Use when tests, metrics, or automated checks are available | Read and apply appropriate techniques from `protocols/discipline.md` to execute applicable methods (tests, similarity metrics, classification accuracy, benchmarks, linting) and document results | In: Evaluation target (required), Evaluation methods (required) | Out: Method results with measurements |
| Evaluate Manually | KR2 | To perform manual inspection and gather evidence | Use when programmatic methods unavailable or human judgment needed | Read and apply appropriate techniques from `protocols/software_quality.md` and `protocols/thinking.md` to inspect target against criteria, request developer evaluation if needed, and document specific observations | In: Evaluation target (required), Criteria (required) | Out: Manual findings with observations |
| Collect and Assess Evidence | KR2 | To list and weigh evidence for all possibilities | Use when evidence gathered and needs assessment | Read and apply appropriate techniques from `protocols/thinking.md` to list evidence for each possible verdict (pass/fail, low/medium/high score), weigh evidence strength, and identify gaps | In: All gathered evidence (required), Criteria (required) | Out: Evidence list for each possibility with weights |
| Report | KR2 | To produce final verdict with supporting evidence | Use when evidence assessed and ready to report | Read and apply appropriate techniques from `protocols/pragmatics.md` to produce verdict, document supporting evidence, state confidence level, and identify strengths/weaknesses | In: Assessed evidence (required) | Out: Verdict with evidence and confidence |
| Provide Recommendations | KR2 | To suggest actionable improvements | Use when recommendations requested or weaknesses identified | Read and apply appropriate techniques from `protocols/thinking.md` and `protocols/pragmatics.md` to provide concrete, prioritized next steps based on evaluation results | In: Evaluation results (required), Context (optional) | Out: Prioritized actionable recommendations |

---

## References

- [Google Engineering Practices - Code Review](https://google.github.io/eng-practices/review/)
- [Google - What to Look For](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [IEEE Standard for Software Verification and Validation](https://standards.ieee.org/ieee/1012/5609/)
- [Better Evaluation - Evaluation Framework](https://www.betterevaluation.org/methods-approaches/methods/evaluation-framework)
