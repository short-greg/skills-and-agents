---
name: verifying
description: >
  Checking whether a solution, hypothesis, or claim matches expectations and/or is correct. Confirms correctness through evidence.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "verify this", "test this", "does this work", "check if correct",
  "validate the result", "run the tests", "confirm this", "is this right".
argument-hint: "[what to verify]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Bash
---

# Verifying

**Goal:** Confirm whether something is correct or matches expectations through evidence.

**Intent:** Catch errors before they propagate. Verification provides confidence that work meets requirements, hypotheses are valid, or claims are true.

**Scope:** Checking correctness against criteria. Answers "is this correct?" or "does this match expectations?" — binary pass/fail, not qualitative assessment.

---

## Key Results - KR

1. Verification criteria established — what "correct" means is defined
2. Evidence gathered — tests run, checks performed, results collected

## Requirements and Constraints - REQ

1. Criteria must be objective and testable
2. Evidence must be documented, not just claimed
3. Distinguish between verified, falsified, and inconclusive
4. Report both passes and failures

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to reason about what evidence would confirm or falsify
- **checklists.md** - Must use to track verification steps systematically
- **tracking.md** - Use when tracking which verifications complete vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** What to verify (solution, hypothesis, claim, result)

**Elicit if not provided:**
- Success criteria (what "correct" means)
- Expected behavior or outcome

**Optional:** Test cases, verification method, acceptance thresholds

## Postconditions

**Success:** Clear verdict (verified/falsified/inconclusive) with supporting evidence

**Failure:** Cannot access what's being verified, criteria undefined, verification method unavailable

---

## Actions

Select based on context. Each action shows which KR it serves.

### Establish Criteria (→ KR1)

Verification criteria must be defined before checking. Determine:
- What constitutes "correct" or "matches expectations"
- What evidence would confirm (pass)
- What evidence would falsify (fail)
- Thresholds for pass/fail if applicable

### Select Verification Method (→ KR2)

Method must match what's being verified:
- **Code/Solution:** Run tests, execute manually, check outputs
- **Hypothesis:** Gather evidence, check predictions, test implications
- **Claim:** Find supporting/contradicting sources, reproduce results
- **Result:** Compare against expected, check calculations, validate format

### Gather Evidence (→ KR2)

Evidence must be collected systematically. Execute verification method, record:
- What was checked
- What was observed
- Pass/fail for each check

### Determine Verdict (→ KR1, KR2)

Verdict must follow from evidence:
- **Verified:** All criteria met, evidence confirms
- **Falsified:** Criteria not met, evidence contradicts
- **Inconclusive:** Insufficient evidence, partial results

---

## Additional Notes and Terms

**Verify vs Critique:** Verify is objective ("does it pass?"). Critique is qualitative ("how good is it?").

**Verify vs Validate:** Often used interchangeably. In this framework, verifying checks correctness against known criteria.

---

## References

- [Software Testing Fundamentals](https://softwaretestingfundamentals.com/)
- [IEEE Standard for Software Verification and Validation](https://standards.ieee.org/ieee/1012/5609/)
