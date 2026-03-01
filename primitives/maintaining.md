---
name: maintaining
description: >
  Keeping a system healthy and well-documented. Addresses accumulated issues and keeps records accurate.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "clean this up", "reduce tech debt", "update dependencies", "fix warnings",
  "housekeeping", "tidy up", "prune unused code", "update the docs", "sync documentation",
  "record this decision", "update the changelog", "maintenance pass".
  keywords: cleaning, updating, syncing, documenting, pruning
argument-hint: "[scope to maintain]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash
---

# Maintaining

**Goal:** Keep system healthy — code clean, dependencies current, documentation accurate.

**Intent:** Prevent accumulated issues from compounding. Without regular maintenance, small problems become large ones and documentation drifts from reality.

**Scope:** Addressing accumulated health issues and keeping records accurate. Includes fixing warnings, removing dead code, updating dependencies, syncing documentation with implementation, and recording decisions. Answers "is this system healthy and well-documented?"

---

## Key Results - KR

1. Health issues addressed — warnings fixed, dead code removed, dependencies updated, or explicitly deferred with rationale
2. Records accurate — documentation matches implementation, decisions recorded, no contradictions

## Requirements and Constraints - REQ

1. Read existing state before making changes
2. Prioritize by impact — security updates first, then quick wins
3. Verify no regressions after changes

---

## Protocols

- **software_quality.md** — Must use for Assess Health, Verify No Regressions
- **discipline.md** — Must use for Assess Health, Triage and Prioritize, Fix Code Health, Verify No Regressions (systematic enumeration)
- **thinking.md** — Must use for Triage and Prioritize (reasoning about severity and effort)
- **risk_management.md** — Must use for Fix Code Health (when changes could break things)
- **transparency.md** — Must use for Create Issue Reports, Sync User Documentation, Sync AI Documentation, Record Decisions
- **instruction_giving.md** — Must use for Sync User Documentation, Sync AI Documentation (clear documentation)
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

Inherits from `base.md` — output lightweight checklist, resolve preconditions, plan actions, execute, report result.

---

## Preconditions

**Required:** Scope — what area to maintain (entire codebase vs specific module)

**Elicit if not provided:**
- Current health indicators (warnings, linter errors, dependency status)
- Documentation locations (where records live for this project)

**Optional:** Known technical debt list, prior maintenance history

## Postconditions

**Success:** Issues addressed or deferred with rationale, records updated, no regressions

**Failure:** Scope too large, cannot assess health, changes would break functionality

---

## Actions

Select actions based on context. Each action shows which KR it serves.

### Assess Health (→ KR1)

Execute health assessment using `software_quality.md` (Quality Dimensions) and `discipline.md` (MECE Enumeration) when beginning maintenance. Systematically enumerate ALL health indicators: linter warnings, compiler warnings, outdated dependencies, dead code, TODOs/FIXMEs. Document current health status with counts.

### Triage and Prioritize (→ KR1)

Execute triage using `thinking.md` (Analytical) and `discipline.md` (Coverage Tracking) when multiple issues exist. Categorize ALL issues by severity and effort systematically. Security issues first, then quick wins, then larger refactors. Document prioritization rationale for each category.

### Fix Code Health (→ KR1)

Execute code fixes using `discipline.md` (Coverage Tracking) and `risk_management.md` (Risk Assessment) when health problems identified. Remove dead code (unused functions, imports, files). Fix warnings (linter, compiler). Update dependencies (prioritize security updates). Assess risk before each change, run tests after each change.

### Create Issue Reports (→ KR2)

Execute issue creation using `transparency.md` (Documentation) when problems found that cannot be fixed now. Document the issue clearly: what's wrong, where it is, potential impact, suggested fix. Create bug reports, technical debt tickets, or enhancement requests as appropriate.

### Sync User Documentation (→ KR2)

Execute user documentation sync using `transparency.md` (Documentation) and `instruction_giving.md` (Explicit Language) when user-facing documentation differs from implementation. Update README, API docs, user guides to match current behavior. Ensure instructions are clear and accurate.

### Sync AI Documentation (→ KR2)

Execute AI documentation sync using `transparency.md` (Documentation) and `instruction_giving.md` (Explicit Language) when AI-facing documentation differs from implementation. Update CLAUDE.md, conventions files, skill definitions to match current patterns. Ensure AI instructions reflect actual project conventions.

### Record Decisions (→ KR2)

Execute decision recording using `transparency.md` (Decision Recording) when significant choices made during maintenance. Document what was decided, why, and alternatives considered. Capture rationale before it's forgotten.

### Verify No Regressions (→ KR1, KR2)

Execute regression check using `software_quality.md` (Quality Dimensions) and `discipline.md` (Coverage Tracking) when changes complete. Run ALL tests systematically, spot-check behavior, verify documentation is accurate. Confirm system still works correctly.

---

## Additional Notes

**Maintenance allocation:** IEEE recommends 15% of development time for refactoring and debt reduction.

**Maintenance vs Orienting:** Orienting assesses current state. Maintaining actively fixes issues and updates records.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Atlassian - Technical Debt Guide](https://www.atlassian.com/agile/software-development/technical-debt)
- [Write the Docs - Documentation Maintenance](https://www.writethedocs.org/guide/)
- [Google - Documentation Best Practices](https://google.github.io/styleguide/docguide/best_practices.html)
