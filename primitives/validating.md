---
name: validating
description: >
  Verifies that output meets success criteria with evidence-based pass/fail verdicts.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "validate this", "does this work", "verify", "test this",
  "does this meet the requirements", "is this right", "acceptance testing".
argument-hint: "[what to validate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Validating

**Goal:** Produce evidence-based verdict on whether work meets success criteria — binary (pass/fail), fuzzy (scale), or categorical (discrete levels).

**Intent:** Prevent shipping broken or incomplete work. Validation catches issues before they reach users or block downstream work.

**Scope:** Verifying outputs against success criteria. Includes code behavior, UI rendering, API responses, documentation accuracy. Answers "does this meet the specified criteria?" not "is this good quality?"

---

## Key Results - KR

1. Each criterion has evidence-based verdict — evidence gathered for BOTH passing AND failing before deciding
2. Overall verdict is pass (all pass) or fail (any fails) with specific gaps identified

## Requirements and Constraints - REQ

1. For each criterion, output evidence for BOTH passing AND failing before deciding (prevents confirmation bias)
2. Flag ambiguous criteria before attempting validation
3. Verify behavior against criteria — use appropriate methods (tests, manual verification, inspection)
4. Output failures with expected vs actual and reproduction steps

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to weigh evidence for pass vs fail (prevents confirmation bias)
- **checklists.md** - Must use to ensure all criteria checked
- **tracking.md** - Use when tracking criteria validated vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** What to validate, success criteria (binary pass/fail criteria)

**Elicit if not provided:**
- Test suite location (if tests exist, run them)
- Requirements or spec (read to understand expected behavior)

**Optional:** Edge case list, prior validation results

## Postconditions

**Success:** All criteria validated with evidence-based verdicts, tests pass, failures documented

**Failure:** Criteria missing or unclear, cannot verify behavior, output fails validation

---

## Actions

Select based on context. Each action shows which KR it serves.

### Gather and Verify Criteria (→ KR1)
Criteria must be verifiable. Collect success criteria. For ambiguous criteria:
1. **Formulate rigorous definition:** Convert vague criteria into observable, measurable conditions with verification methods
2. **Validate against definition:** Once rigorous, proceed with validation

Verify each criterion type (binary pass/fail, fuzzy scale, or categorical levels). Flag criteria that cannot be made rigorous.

### Check Each Criterion (→ KR1)
Evidence must be documented for multiple possible outcomes. For each criterion:
1. **Get realistic outputs:** For UI, capture screenshots or rendered output. For APIs, get actual responses. For behavior, observe actual execution.
2. **Output evidence for different outcomes:** For binary (evidence for pass, evidence for fail). For fuzzy (measure actual value). For categorical (evidence for each level).
3. Only after outputting evidence, weigh and decide verdict
4. Output verdict with reasoning

Techniques: automated tests, manual testing, UI rendering verification, boundary testing, error cases, integration testing.

### Compute Overall Verdict (→ KR2)
Overall must reflect all criteria. Pass only if ALL pass. Fail if ANY fails. For failures: what failed, expected vs actual, reproduction steps.

---

## Additional Notes and Terms

**Validation vs Critique:** Validation is objective ("meets criteria?"). Critique is qualitative ("how good?").

**Rigorous criteria:** A criterion is validatable when it specifies: (1) observable behavior or state, (2) unambiguous threshold or condition, (3) method to verify.

**Criterion types:**
- **Binary:** Pass/fail. Example: "All tests pass" or "Response time < 200ms"
- **Fuzzy:** Continuous scale. Example: "Code coverage: 73%" or "Performance score: 0.85/1.0"
- **Categorical:** Discrete levels. Example: "Security risk: low/medium/high" or "Quality: poor/fair/good/excellent"

Vague examples: "Fast enough", "good quality" (not verifiable without thresholds).

**Two-step pattern when criteria are vague:** (1) Formulate rigorous definition — convert vague criteria into observable, measurable conditions. (2) Validate against that definition — once rigorous, gather evidence for pass/fail.

**Falsification principle (Popper):** Seek evidence that would disprove, not just confirm. A single genuine counter-instance falsifies. This is why REQ1 requires evidence for BOTH passing AND failing.

**UI validation:** For UI criteria, get realistic outputs. Run the application, capture screenshots, or render components. Don't just inspect code — verify what users actually see. UI validation requires observing actual rendered output against visual/behavioral criteria.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Karl Popper - Falsification Theory](https://plato.stanford.edu/entries/popper/)
- [Farnam Street - Confirmation Bias and Disconfirming Evidence](https://fs.blog/confirmation-bias/)
- [Virtuoso QA - Acceptance Testing Best Practices](https://www.virtuosoqa.com/testing-guides/what-is-acceptance-testing)
