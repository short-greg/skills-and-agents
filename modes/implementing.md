---
name: implementing
description: >
  Realizing a solution. Writes code, tests, or other artifacts to realize a design.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
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

## Steps

MUST read and follow steps in `base_mode.md`

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

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Gather Context (→ KR1)

**Goal:** Clarify what needs to be implemented

**When:** Bug report is unclear, screenshot needs interpretation, error message needs analysis, or reproduction steps are missing

**Protocols:** `protocols/elicitation.md`, `protocols/pragmatics.md`

**Instructions:** Use interviewing techniques to elicit information from user. Apply pragmatic communication patterns to ask for clarification effectively.

**Inputs:**
- Bug report, screenshot, error message, or unclear requirement (required)
- Project context (optional)

**Default Output:** Clear understanding of what needs to be implemented

### Prepare (→ KR1)

**Goal:** Set up foundation for implementation

**When:** Starting implementation

**Protocols:** `protocols/discipline.md`, `protocols/system_modularity.md`

**Instructions:** Review conventions, read existing code patterns, review design, set up scaffolding and file structure. Use systematic coverage tracking and apply modularity principles.

**Inputs:**
- Project conventions (required)
- Existing codebase (required)
- Design specification (optional)

**Default Output:** File structure and scaffolding ready for implementation

---

### Write Stubs (→ KR1)

**Goal:** Define interfaces and structure before implementation

**When:** Implementation is complex and structure should be reviewed before filling in logic

**Protocols:** `protocols/system_modularity.md`, `protocols/criteria_setting.md`

**Instructions:** Define function signatures, interfaces, and module boundaries first. Apply information hiding and specify observable behavior for each interface.

**Inputs:**
- Design specification (required)
- Module boundaries (optional)

**Default Output:** Interface definitions and function signatures

---

### Write Code (→ KR1)

**Goal:** Write functional code that meets requirements

**When:** Writing or modifying code

**Protocols:** `protocols/system_modularity.md`, `protocols/criteria_setting.md`, `protocols/transparency.md`

**Instructions:** Write modular code with appropriate documentation, integrate with existing modules and APIs, verify against design criteria. Apply cohesion and coupling principles, document functions and modules appropriately. Flag design issues if discovered.

**Inputs:**
- Requirements or design (required)
- Existing codebase (required)
- Interface definitions (optional)
- Documentation standards (optional)

**Default Output:** Working code with appropriate documentation that implements specified functionality

### Write Tests (→ KR2)

**Goal:** Create comprehensive test coverage

**When:** New functionality needs test coverage

**Protocols:** `protocols/discipline.md`, `protocols/criteria_setting.md`, `protocols/risk_management.md`

**Instructions:** Enumerate ALL edge cases and error paths systematically, identify implementation risks and failure modes, write unit and integration tests. Use MECE enumeration, coverage tracking, and risk assessment to ensure completeness.

**Inputs:**
- Implemented functionality (required)
- Requirements or design (required)
- Existing test patterns (optional)
- Known risks (optional)

**Default Output:** Unit and integration tests covering all edge cases and identified risks

---

### Run Tests (→ KR2)

**Goal:** Verify implementation correctness

**When:** Verifying implementation works

**Protocol:** `protocols/software_quality.md`

**Instructions:** Run all tests including existing tests, check for regressions, document failures with specifics. Assess against quality dimensions.

**Inputs:**
- Test suite (required)
- Implemented code (required)

**Default Output:** Test results with pass/fail status and failure details

---

### Self-Review (→ KR1, KR2)

**Goal:** Ensure implementation quality and completeness

**When:** Implementation is complete

**Protocols:** `protocols/software_quality.md`, `protocols/transparency.md`

**Instructions:** Review code for quality and correctness, verify conventions followed, document API changes if any. Assess against quality dimensions and document changes appropriately.

**Inputs:**
- Completed implementation (required)
- Project conventions (required)
- Original requirements (required)

**Default Output:** Quality assessment and updated documentation if needed

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Google - Code Review Standards](https://google.github.io/eng-practices/review/reviewer/standard.html)
