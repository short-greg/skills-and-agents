---
name: implementing
description: >
  Realizing a solution. Writes code, tests, or other artifacts to realize a design.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "implement this", "write the code", "build this", "code this up",
  "create the feature", "add the tests", "implement the design", "write the function".
  keywords: coding, building, writing, testing, integrating
argument-hint: "[what to implement]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Implementing

**Goal:** Produce working code or artifacts that realize design and meet specified requirements.

**Intent:** Turn plans and designs into reality. Good implementation follows design, fits project conventions, handles edge cases, and produces code that is correct, readable, and maintainable.

**Scope:** Writing and modifying code, tests, configuration, and other artifacts. Includes translating designs into code, following project conventions and patterns, handling error cases and edge cases, writing tests alongside implementation, integrating with existing code, and iterating based on feedback. Implementation answers "does the code exist and work?" not "is it the right approach?"

---

## Key Results - KR

1. Implementation matches design and requirements — follows conventions, handles edge cases, code is readable, integrates cleanly
2. Tests pass — or failures documented with specifics

## Requirements and Constraints - REQ

1. Understand project conventions BEFORE writing any code
2. Produce modular code with clear boundaries and minimal coupling
3. Update documentation when implementation changes behavior or APIs

---

## Protocols

- **interviewing.md** — Use for Gather Context (when bug report, screenshot, error message needs clarification)
- **pragmatics.md** — Use for Gather Context (when asking for clarification)
- **discipline.md** — Must use for Prepare, Write Tests (systematic coverage)
- **system_modularity.md** — Must use for Prepare, Write Stubs, Implement (modular code boundaries)
- **criteria_setting.md** — Must use for Write Stubs, Implement, Write Tests (verify matches design)
- **software_quality.md** — Must use for Run Tests, Self-Review (quality verification)
- **transparency.md** — Use for Self-Review (when changes affect behavior/APIs)
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

MUST read and follow steps in `base.md`

---

## Preconditions

**Required:** What to implement, design or approach (how it should be built), project conventions (repo patterns, style, structure)

**Elicit if not provided:**
- Existing codebase (understand what exists and how to integrate)
- Dependencies (understand available libraries, APIs, modules)

**Optional:** Prior implementation (if exists, read as starting point), tests to satisfy, examples (similar code to use as reference)

## Postconditions

**Success:** Code implements specified functionality, matches design, follows conventions, handles edge cases, tests pass

**Failure:** Requirements unclear, design missing, blocked by external factors

---

## Possible Actions

Select based on context. Each action shows which KR it serves.

### Gather Context (→ KR1)

Execute context gathering using `interviewing.md` (Open Questions, Inference First) and `pragmatics.md` (Directness Calibration) when bug report is unclear, screenshot needs interpretation, error message needs analysis, or reproduction steps are missing. Clarify what needs to be implemented before proceeding.

### Prepare (→ KR1)

Execute preparation using `discipline.md` (Coverage Tracking) and `system_modularity.md` (Cohesion, Coupling) when starting implementation. Review conventions, read existing code patterns, review design, set up scaffolding and file structure.

### Write Stubs (→ KR1)

Execute stub creation using `system_modularity.md` (Information Hiding, Interfaces) and `criteria_setting.md` (Observable Behavior) when implementation is complex and structure should be reviewed before filling in logic. Define function signatures, interfaces, and module boundaries first.

### Implement (→ KR1)

Execute implementation using `system_modularity.md` (Cohesion, Coupling) and `criteria_setting.md` (Observable Behavior) when writing or modifying code. Write modular code, integrate with existing modules and APIs, verify against design criteria. Flag design issues if discovered.

### Write Tests (→ KR2)

Execute test creation using `discipline.md` (MECE Enumeration, Coverage Tracking) and `criteria_setting.md` (Observable Behavior) when new functionality needs test coverage. Enumerate ALL edge cases and error paths systematically, write unit and integration tests.

### Run Tests (→ KR2)

Execute test verification using `software_quality.md` (Quality Dimensions) when verifying implementation works. Run all tests including existing tests, check for regressions, document failures with specifics.

### Self-Review (→ KR1, KR2)

Execute self-review using `software_quality.md` (Quality Dimensions) and `transparency.md` (Documentation) when implementation is complete. Review code for quality and correctness, verify conventions followed, document API changes if any.

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Google - Code Review Standards](https://google.github.io/eng-practices/review/reviewer/standard.html)
