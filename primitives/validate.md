---
name: validate
description: >
  Use when verifying that an output meets its requirements and works correctly. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "validate this", "does this work", "check if this is correct", "verify", "test this", "does this meet the requirements", "is this right", "confirm this works", "QA this", "acceptance testing".
argument-hint: "[what to validate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Validate

**Goal:** Produce binary pass/fail verdict on whether work meets success criteria.

**Intent:** Prevent shipping broken or incomplete work. Validation catches issues before they reach users or block downstream work.

**Scope:** Verifying outputs against success criteria by running tests, checking behavior against acceptance criteria, verifying edge cases and error handling, and producing binary pass/fail verdict for each criterion. Validation is objective verification — it answers "does this meet the specified criteria?" not "is this good quality?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Each success criterion has binary pass/fail verdict with evidence for both passing and failing before deciding
2. Overall verdict is pass (all criteria pass) or fail (any criterion fails) with specific gaps identified

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. For each criterion, find evidence BOTH for passing AND for failing, then decide based on weight of evidence
2. Flag ambiguous criteria before attempting validation
3. Track which criteria validated vs remain per `tracking.md`
4. Run tests and verify behavior, not just inspect code
5. Document failures with expected vs actual and reproduction steps

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** What to validate, success criteria (binary pass/fail criteria from definition)

**Elicit if not provided:**
- Test suite location (if tests exist, run them)
- Requirements or spec (read to understand expected behavior)

**Optional:** Edge case list, prior validation results

## Postconditions

The resulting state after the primitive is finished.

**Success:** All criteria validated with evidence-based verdicts, tests pass, failures documented with specifics.

**Failure:** Criteria missing or unclear, cannot run tests or verify behavior, output fails validation.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Gather and Verify Criteria (→ KR1)

Collect success criteria from definition. Verify each criterion is verifiable as pass/fail. Flag any ambiguous criteria before proceeding.

### Check Each Criterion (→ KR1)

For each success criterion:
1. Find evidence the criterion PASSES (why it meets the requirement)
2. Find evidence the criterion FAILS (why it doesn't meet the requirement)
3. Decide PASS or FAIL based on weight of evidence
4. Document the verdict with specific evidence

Use verification techniques: run automated tests, manual testing, boundary testing, error case testing, integration testing, regression testing.

### Compute Overall Verdict (→ KR2)

Pass only if ALL criteria pass. Fail if ANY criterion fails. For failures, identify specific gaps — what failed, expected vs actual, reproduction steps.

---

## Additional Notes and Terms

**Validation is objective verification** — answers "does this meet criteria?" not "is this good quality?" For quality assessment, use `critique` primitive.

**Evidence-based decisions** — REQ1 requires finding evidence for both passing and failing before deciding. This prevents confirmation bias and ensures rigorous evaluation.


[TODO: Include information on how to define a term rigorously so that it can be validated whether it is satisfied]