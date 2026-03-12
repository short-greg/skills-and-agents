---
name: maintaining
description: >
  Keeping a system healthy and well-documented. Addresses accumulated issues and keeps records accurate.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "clean this up", "reduce tech debt", "update dependencies", "fix warnings",
  "housekeeping", "tidy up", "prune unused code", "update the docs", "sync documentation",
  "record this decision", "update the changelog", "maintenance pass".
  keywords: cleaning, updating, syncing, documenting, pruning
---

# Maintaining

**Goal:** Keep system healthy — code clean, dependencies current, documentation accurate.

**Intent:** Prevent accumulated issues from compounding. Without regular maintenance, small problems become large ones and documentation drifts from reality.

**Scope:** Addressing accumulated health issues and keeping records accurate. Answers "is this system healthy and well-documented?" Quick fixes only — identifies but does not execute heavy refactoring.

---

## Key Results - KR

1. Health issues addressed — warnings fixed, dead code removed, dependencies updated, or deferred with rationale
2. Records accurate — documentation matches implementation and standards, decisions recorded

---

## Preconditions

**Required:** Scope — what area to maintain (entire codebase vs specific module)

**Optional:**
- Prioritized issue list (from orienting/evaluating)
- Current health indicators (warnings, linter errors, dependency status)
- Documentation locations (where records live for this project)

## Postconditions

**Success:** Issues addressed or deferred with rationale, records updated, no regressions

**Failure:** Scope too large, cannot assess health, changes would break functionality. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Fix Code Issues | KR1 | To address quick-fix health problems | Use when warnings, dead code, or security vulnerabilities exist | Read `protocols/discipline.md` and apply appropriate techniques to fix warnings, remove dead code, and patch security issues. Run tests after each change | In: Issues to fix (required), Test suite (required) | Out: Fixed code with no regressions |
| Update Dependencies | KR1 | To keep packages current and secure | Use when dependencies are outdated or have security patches | Read `protocols/risk_management.md` and apply appropriate techniques to update packages, prioritizing security patches. Test after updates | In: Dependency manifest (required), Test suite (required) | Out: Updated dependencies with no regressions |
| Update Documentation | KR2 | To keep docs accurate and current | Use when documentation differs from implementation or new standards apply | Read `protocols/transparency.md` and `protocols/instruction_giving.md` and apply appropriate techniques to sync docs with implementation and adapt to new standards/practices | In: Current implementation (required), Documentation files (required), New standards (optional) | Out: Updated documentation |
| Record Issues and Decisions | KR2 | To document unfixed problems and decisions made | Use when problems cannot be fixed now or significant decisions made | Read `protocols/transparency.md` and apply appropriate techniques to document issues (including refactoring needs) as technical debt and record decisions with rationale | In: Issues or decisions (required), Context (optional) | Out: Issue reports and decision records |
| Verify Changes | KR1, KR2 | To confirm no regressions from maintenance | Use when maintenance changes complete | Read `protocols/software_quality.md` and apply appropriate techniques to run tests, spot-check behavior, and verify documentation accuracy | In: Modified code (required), Test suite (required), Documentation (required) | Out: Verification report with pass/fail status |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Software Maintenance Best Practices 2024 - Hyperping](https://hyperping.com/blog/software-maintenance-best-practices-for-2024)
- [Software Maintenance Types - TeachingAgile](https://teachingagile.com/sdlc/maintenance)
- [Atlassian - Technical Debt Guide](https://www.atlassian.com/agile/software-development/technical-debt)
