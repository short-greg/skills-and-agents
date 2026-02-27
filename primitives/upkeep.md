---
name: upkeep
description: >
  Maintaining quality and reducing friction of a system or project. Keeps system healthy by addressing accumulated issues before they compound.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "clean this up", "reduce tech debt", "update dependencies",
  "fix warnings", "housekeeping", "tidy up", "prune unused code".
argument-hint: "[what to clean up or maintain]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Upkeep

**Goal:** Keep system healthy by addressing accumulated issues — warnings, dead code, outdated dependencies, technical debt.

**Intent:** Prevent slow accumulation of problems that makes systems harder to work with. Without regular upkeep, small issues compound into large ones.

**Scope:** Addressing accumulated health issues. Answers "is this clean and sustainable?" not "does it work right now?"

---

## Key Results - KR

1. Health issues identified and addressed — warnings fixed, dead code removed, dependencies updated, or explicitly deferred with rationale
2. No regressions — existing functionality preserved, tests pass

## Requirements and Constraints - REQ

1. Allocate focused time — don't let upkeep be squeezed out by features
2. Prioritize by impact — security updates first, then quick wins, then larger refactors
3. Verify no regressions after each change

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **quality.md** - Must use to assess and improve health dimensions
- **reasoning.md** - Must use to prioritize tasks
- **checklists.md** - Must use to ensure systematic coverage
- **documentation.md** - Use when upkeep changes behavior or removes features
- **tracking.md** - Use when tracking tasks complete vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Scope — what area to address (entire codebase vs specific module)

**Elicit if not provided:**
- Current health indicators (warnings, linter errors, test coverage, dependency status)
- Project conventions (what "healthy" looks like for this project)

**Optional:** Known technical debt list, prior upkeep history

## Postconditions

**Success:** Issues addressed or deferred with rationale, no regressions, system healthier

**Failure:** Scope too large, cannot assess health, changes would break functionality

---

## Actions

Select based on context. Each action shows which KR it serves.

### Assess Health (→ KR1)
Current state must be known. Run linters, check warnings, identify outdated dependencies, find dead code, review TODOs/FIXMEs.

### Triage and Prioritize (→ KR1)
Issues must be prioritized. Categorize by severity and effort. Security issues first, then quick wins.

### Remove Dead Code (→ KR1)
Unused code must be removed. Delete unused functions, imports, files, dependencies.

### Update Dependencies (→ KR1)
Dependencies must be current. Upgrade outdated packages, prioritize security updates.

### Fix Warnings (→ KR1)
Warnings must be addressed. Fix linter and compiler warnings systematically.

### Verify No Regressions (→ KR2)
Functionality must be preserved. Run tests after each change, spot-check behavior.

---

## Additional Notes and Terms

**Maintenance allocation:** IEEE recommends 15% of development time for refactoring and debt reduction.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Atlassian - Technical Debt Guide](https://www.atlassian.com/agile/software-development/technical-debt)
- [Monday.com - Technical Debt Strategic Guide 2026](https://monday.com/blog/rnd/technical-debt/)
- [IBM - What Is Preventive Maintenance](https://www.ibm.com/think/topics/what-is-preventive-maintenance)
