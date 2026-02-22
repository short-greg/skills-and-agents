---
name: validate
description: >
  Use when verifying that an output meets its requirements and works correctly.
  Triggers on: "validate this", "does this work", "check if this is correct",
  "verify", "test this", "does this meet the requirements", "is this right",
  "confirm this works", "QA this", "acceptance testing".
argument-hint: "[what to validate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Validate

**Goal:** Produce a binary pass/fail verdict on whether work is complete by checking each success criterion.

**Intent:** Prevent shipping broken or incomplete work. Validation catches issues before they reach users or block downstream work. It answers the question: "Is this done?" — a yes/no answer, not a quality judgment.

**Scope:** Determining whether work is done by verifying outputs against SMARB success criteria. Includes: running tests and checking results, verifying behavior against acceptance criteria, checking edge cases and error handling, confirming integration works, and producing a binary pass/fail verdict for each criterion. When multiple criteria exist, each is evaluated independently and the overall verdict passes only if all criteria pass. Validation is objective verification — it answers "does this meet the specified criteria?" not "is this good quality?"

---

## Key Results

1. Each success criterion receives a binary pass/fail verdict with evidence
2. Overall verdict is pass (all criteria pass) or fail (any criterion fails)
3. Tests pass (or failures are documented with specifics)
4. Edge cases enumerated in criteria are verified
5. If validation fails, specific gaps are identified — not just "it doesn't work"
6. If criteria are ambiguous or not verifiable as pass/fail, this is flagged before attempting validation

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which criteria have been validated vs remain
- `recovery.md` — Resume validation from where it was interrupted
- `reasoning.md` — Reason about validation approach before starting, verify all criteria checked after
- `goals_and_objectives.md` — Validate against outcome-oriented success criteria
- `quality.md` — Check against quality dimensions: correctness, clarity, reliability
- `documentation.md` — Flag documentation gaps or inaccuracies discovered during validation

---

## Preconditions

**Must be provided:**
- what to validate: the output, feature, or artifact being checked — ask if not clear
- SMARB success criteria: binary pass/fail criteria from definition — ask if not available or if criteria are ambiguous

**Self-satisfiable:**
- test suite: if tests exist, run them
- requirements: read definition or spec to understand what was supposed to be built

**Non-essential:**
- edge case list: if design enumerated edge cases, use them as a validation checklist
- prior validation results: if this was validated before, focus on what changed

---

## Postconditions

**Success:**
- Output is confirmed to meet all success criteria
- Tests pass
- Edge cases are handled correctly
- Integration works as expected
- Any gaps or failures are explicitly documented

**Failure:**
- Success criteria are missing or unclear — cannot validate without knowing what "correct" means
- Cannot run tests or verify behavior (environment issues, missing access)
- Output fails validation — specific failures are documented

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Preparation**
- **gather success criteria**: collect SMARB criteria from definition
- **verify criteria are binary**: each criterion must be verifiable as pass/fail; flag any that are ambiguous
- **identify test cases**: enumerate what needs to be checked — happy path, edge cases, error cases
- **review edge case list**: if design enumerated edge cases, use them as a checklist

**Verification Techniques**
- **run automated tests**: execute test suite and check results
- **manual testing**: manually exercise the feature or output
- **boundary testing**: test edge cases and boundary conditions
- **error case testing**: verify error handling works as specified
- **integration testing**: verify the output works within the larger system
- **regression testing**: verify existing functionality still works

**Evaluation**
- **check against criteria**: for each success criterion, determine pass or fail with concrete evidence
- **document failures**: for any failure, record what failed, expected vs. actual, and how to reproduce
- **compute overall verdict**: pass only if ALL criteria pass; fail if ANY criterion fails
- **assess completeness**: is everything that was supposed to be built actually built?

**Reporting**
- **summarize results**: overall pass/fail verdict with breakdown showing each criterion's pass/fail status
- **list gaps**: specific issues that need to be addressed
- **recommend next steps**: if validation failed, what needs to happen to fix it

---

## Confirm

Before declaring done, verify against each key result:
- Was each success criterion checked with a binary pass/fail verdict?
- Are tests passing?
- Were edge cases verified?
- Is the validation complete?

Report outcome explicitly: state the overall verdict (PASS or FAIL), list each criterion with its individual verdict, and explain why.

---

## Use Cases

- After implementation, before considering a feature complete
- Acceptance testing before release
- Verifying a bug fix actually fixes the bug
- Checking that refactoring didn't break functionality
- Confirming integration works after connecting systems

---

## Tools

Bash

## Hooks

None
