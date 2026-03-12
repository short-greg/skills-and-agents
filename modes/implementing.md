---
name: implementing
description: >
  Realizing a solution. Writes code, tests, or other artifacts to realize a design.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "implement this", "write the code", "build this", "code this up",
  "create the feature", "add the tests", "implement the design", "write the function".
  keywords: coding, building, writing, testing, integrating
---

# Implementing

**Goal:** Produce working code or artifacts that realize design and meet specified requirements.

**Intent:** Turn plans and designs into reality. Good implementation follows design, fits project conventions, handles edge cases, and produces code that is correct, readable, and maintainable.

**Scope:** Writing and modifying code, tests, configuration, and other artifacts. Answers "does the code exist and work?" not "is it the right approach?"

---

## Key Results - KR

1. Implementation matches design and requirements — follows conventions, handles edge cases, code is readable, integrates cleanly
2. Tests pass — or failures documented with specifics

---

## Preconditions

**Required:** What to implement, design or approach (how it should be built), project conventions (repo patterns, style, structure)

**Optional:**
- Existing codebase (understand what exists and how to integrate)
- Dependencies (understand available libraries, APIs, modules)
- Prior implementation (if exists, read as starting point)
- Tests to satisfy, examples (similar code to use as reference)

## Postconditions

**Success:** Code implements specified functionality, matches design, follows conventions, handles edge cases, tests pass

**Failure:** Requirements unclear, design missing, blocked by external factors. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Gather Context | KR1 | To clarify what needs to be implemented | Use when bug report is unclear, screenshot needs interpretation, error message needs analysis, or reproduction steps are missing | Read `protocols/elicitation.md` and `protocols/pragmatics.md` and apply appropriate techniques to elicit missing information | In: Bug report, screenshot, error message, or unclear requirement (required), Project context (optional) | Out: Clear understanding of what needs to be implemented |
| Analyze Existing Code | KR1 | To understand what exists before modifying | Use when integrating with existing code or modifying existing functionality | Read `protocols/thinking.md` and apply appropriate techniques to trace code paths, identify dependencies, and understand current behavior | In: Relevant code files (required), Design specification (optional) | Out: Understanding of existing structure, dependencies, and integration points |
| Write Code | KR1 | To produce functional code that meets requirements | Use when writing or modifying code | Read `protocols/system_modularity.md` and `protocols/criteria_setting.md` and apply appropriate techniques to write modular code with clear boundaries | In: Requirements or design (required), Existing codebase (required), Interface definitions (optional) | Out: Working code that implements specified functionality |
| Simplify | KR1 | To reduce complexity and combat bloat | Use when implementation is complete and code can be made cleaner, or when noticing unnecessary complexity during implementation | Read `protocols/system_modularity.md` and `protocols/thinking.md` and apply appropriate techniques to identify and remove unnecessary code, abstractions, or indirection | In: Implemented code (required), Original requirements (required) | Out: Simplified code with reduced complexity |
| Write Tests | KR2 | To create comprehensive test coverage | Use when new functionality needs test coverage | Read `protocols/discipline.md` and `protocols/criteria_setting.md` and apply appropriate techniques to enumerate edge cases and error paths systematically | In: Implemented functionality (required), Requirements or design (required), Existing test patterns (optional) | Out: Unit and integration tests covering edge cases |
| Run and Verify | KR2 | To verify implementation correctness | Use when verifying implementation works | Read `protocols/software_quality.md` and apply appropriate techniques to run tests, check for regressions, and document failures | In: Test suite (required), Implemented code (required) | Out: Test results with pass/fail status and failure details |
| Self-Review | KR1, KR2 | To ensure implementation quality and completeness | Use when implementation is complete | Read `protocols/software_quality.md` and `protocols/system_modularity.md` and apply appropriate techniques to assess code quality, modularity, and maintainability | In: Completed implementation (required), Project conventions (required), Original requirements (required) | Out: Quality assessment with modularity evaluation |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Google - Code Review Standards](https://google.github.io/eng-practices/review/reviewer/standard.html)
