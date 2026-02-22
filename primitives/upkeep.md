---
name: upkeep
description: >
  Use when maintaining system health and preventing degradation. Triggers on:
  "clean this up", "maintain this", "keep this healthy", "prevent rot",
  "reduce tech debt", "improve code health", "housekeeping", "tidy up",
  "prune unused", "update dependencies", "fix warnings".
argument-hint: "[what to maintain or clean up]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Upkeep

**Goal:** Maintain system health and prevent gradual degradation — keep things working well over time.

**Intent:** Prevent the slow accumulation of problems that makes systems harder to work with. Without regular upkeep, technical debt grows, dependencies become outdated, warnings accumulate, unused code clutters the codebase, and small issues compound into large ones.

**Scope:** Keeping things healthy and functional over time. Includes: removing dead code and unused dependencies, updating outdated dependencies, fixing warnings and linter errors, reducing technical debt, improving test coverage, cleaning up temporary workarounds, and addressing small issues before they become large ones. Upkeep is about system health — it answers "is this maintainable and sustainable?" not "do our records match reality?"

---

## Key Results

1. Health issues are identified with specific locations and severity
2. Issues are addressed or explicitly deferred with rationale
3. No regressions — existing functionality still works
4. Changes follow project conventions
5. System is measurably healthier (fewer warnings, updated deps, less dead code)

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which maintenance tasks are complete vs remain
- `recovery.md` — Resume upkeep from where it was interrupted
- `reasoning.md` — Reason about maintenance priorities before starting, verify health improvements after
- `quality.md` — Assess and improve quality dimensions: reduce warnings, update deps, remove dead code
- `documentation.md` — Update documentation when upkeep changes behavior or removes features

---

## Preconditions

**Must be provided:**
- scope of upkeep: what area or aspect to maintain — ask if not clear (entire codebase vs. specific module)

**Self-satisfiable:**
- current health indicators: warnings, linter errors, test coverage, dependency status
- project conventions: understand what "healthy" looks like for this project

**Non-essential:**
- known technical debt: if tracked, use as a starting point
- prior upkeep: understanding what was addressed recently helps prioritize

---

## Postconditions

**Success:**
- Identified issues are addressed or explicitly deferred with rationale
- No new issues were introduced
- System is healthier than before
- Changes are safe — functionality is preserved

**Failure:**
- Scope is too large to address meaningfully — needs scoping
- Cannot assess health (missing tooling, no tests)
- Changes would break functionality and cannot be made safely

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Assessment**
- **check warnings**: run linters, compilers, static analysis — surface existing warnings
- **check dependencies**: identify outdated, deprecated, or vulnerable dependencies
- **check test coverage**: identify untested or under-tested areas
- **identify dead code**: find unused functions, imports, files, dependencies
- **review TODOs and FIXMEs**: check for deferred work that should be addressed
- **assess complexity**: identify overly complex or hard-to-maintain areas

**Prioritization**
- **triage issues**: categorize by severity (critical, moderate, minor) and effort (quick win, moderate, large)
- **identify quick wins**: low-effort, high-impact improvements
- **identify safety boundaries**: what can be changed safely vs. what needs careful testing

**Remediation**
- **remove dead code**: delete unused functions, imports, files
- **update dependencies**: upgrade outdated packages — prioritize security updates
- **fix warnings**: address linter and compiler warnings
- **clean up workarounds**: remove temporary fixes that are no longer needed
- **simplify complexity**: refactor overly complex code
- **improve test coverage**: add tests to critical but untested paths

**Verification**
- **run tests**: ensure no regressions from upkeep changes
- **verify functionality**: spot-check that things still work
- **check for new issues**: ensure upkeep didn't introduce new problems

---

## Confirm

Before declaring done, verify against each key result:
- Were health issues identified with locations and severity?
- Were issues addressed or explicitly deferred?
- Were regressions avoided?
- Is the system healthier?

Report outcome explicitly: state what was improved, what was deferred, and overall health impact.

---

## Use Cases

- Regular codebase maintenance
- Preparing for a major release
- Onboarding to a new codebase (clean up as you learn)
- After completing a feature (clean up before moving on)
- Addressing accumulated technical debt
- Security maintenance (dependency updates)

---

## Tools

Write, Edit, Bash

## Hooks

None
