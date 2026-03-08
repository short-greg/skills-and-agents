---
name: maintaining
description: >
  Keeping a system healthy and well-documented. Addresses accumulated issues and keeps records accurate.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "clean this up", "reduce tech debt", "update dependencies", "fix warnings",
  "housekeeping", "tidy up", "prune unused code", "update the docs", "sync documentation",
  "record this decision", "update the changelog", "maintenance pass".
  keywords: cleaning, updating, syncing, documenting, pruning
---

# Maintaining

**Goal:** Keep system healthy — code clean, dependencies current, documentation accurate.

**Intent:** Prevent accumulated issues from compounding. Without regular maintenance, small problems become large ones and documentation drifts from reality.

**Scope:** Addressing accumulated health issues and keeping records accurate. Includes fixing warnings, removing dead code, updating dependencies, syncing documentation with implementation, and recording decisions. Answers "is this system healthy and well-documented?"

---

## Table of Contents

- [Key Results](#key-results---kr) — Success criteria for this mode
- [Requirements](#requirements-and-constraints---req) — Rules and constraints to follow
- [Steps](#steps) — Reference to base mode execution
- [Terms](#terms) — Key vocabulary and definitions
- [Preconditions](#preconditions) — What's needed before starting
- [Postconditions](#postconditions) — What's delivered upon completion
- [Actions](#possible-actions) — Concrete steps to achieve results
- [Notes](#additional-notes-and-terms) — Additional context and details
- [References](#references) — External documentation and resources

## Key Results - KR

1. Health issues addressed — warnings fixed, dead code removed, dependencies updated, or explicitly deferred with rationale
2. Records accurate — documentation matches implementation, decisions recorded, no contradictions

## Requirements and Constraints - REQ

1. Read existing state before making changes
2. Prioritize by impact — security updates first, then quick wins
3. Verify no regressions after changes

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** Scope — what area to maintain (entire codebase vs specific module)

**Optional:**
- Current health indicators (warnings, linter errors, dependency status)
- Documentation locations (where records live for this project)
- Known technical debt list, prior maintenance history

## Postconditions

**Success:** Issues addressed or deferred with rationale, records updated, no regressions

**Failure:** Scope too large, cannot assess health, changes would break functionality. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Assess Health (→ KR1)

**Goal:** Document current system health status

**When:** Beginning maintenance

**Protocols:** `protocols/software_quality.md`, `protocols/discipline.md`, `protocols/system_modularity.md`

**Instructions:** Systematically enumerate ALL health indicators: linter warnings, compiler warnings, outdated dependencies, dead code, TODOs/FIXMEs, coupling issues, cohesion problems. Document current health status with counts. Apply quality dimensions, modularity assessment, and MECE enumeration.

**Inputs:**
- Codebase or scope to assess (required)
- Project tooling configuration (optional)
- Modularity standards (optional)

**Default Output:** Health report with counts for each indicator type including modularity assessment

### Triage and Prioritize (→ KR1)

**Goal:** Determine what to fix and in what order

**When:** Multiple issues exist

**Protocols:** `protocols/thinking.md`, `protocols/discipline.md`

**Instructions:** Categorize ALL issues by severity and effort systematically. Security issues first, then quick wins, then larger refactors. Document prioritization rationale for each category. Use analytical thinking and coverage tracking.

**Inputs:**
- Health assessment results (required)
- Project priorities (optional)

**Default Output:** Prioritized list of issues with rationale

---

### Fix Code Health (→ KR1)

**Goal:** Address health problems

**When:** Health problems identified

**Protocols:** `protocols/discipline.md`, `protocols/risk_management.md`

**Instructions:** Remove dead code (unused functions, imports, files). Fix warnings (linter, compiler). Update dependencies (prioritize security updates). Assess risk before each change, run tests after each change. Track coverage and assess risks systematically.

**Inputs:**
- Prioritized issue list (required)
- Test suite (required)
- Dependency manifest (optional)

**Default Output:** Fixed code with no regressions

---

### Create Issue Reports (→ KR2)

**Goal:** Document problems that cannot be fixed now

**When:** Problems found that cannot be fixed now

**Protocol:** `protocols/transparency.md`

**Instructions:** Document the issue clearly: what's wrong, where it is, potential impact, suggested fix. Create bug reports, technical debt tickets, or enhancement requests as appropriate. Apply documentation techniques.

**Inputs:**
- Identified issues (required)
- Project tracking system (optional)

**Default Output:** Issue reports in appropriate format

### Sync User Documentation (→ KR2)

**Goal:** Ensure user documentation matches implementation

**When:** User-facing documentation differs from implementation

**Protocols:** `protocols/transparency.md`, `protocols/instruction_giving.md`

**Instructions:** Update README, API docs, user guides to match current behavior. Ensure instructions are clear and accurate. Apply documentation techniques and explicit language.

**Inputs:**
- Current implementation (required)
- Documentation files (required)

**Default Output:** Updated user documentation

---

### Sync AI Documentation (→ KR2)

**Goal:** Ensure AI documentation matches implementation

**When:** AI-facing documentation differs from implementation

**Protocols:** `protocols/transparency.md`, `protocols/instruction_giving.md`

**Instructions:** Update CLAUDE.md, conventions files, skill definitions to match current patterns. Ensure AI instructions reflect actual project conventions. Apply documentation techniques and explicit language.

**Inputs:**
- Current implementation (required)
- AI documentation files (required)

**Default Output:** Updated AI documentation

---

### Record Decisions (→ KR2)

**Goal:** Capture significant decisions before they're forgotten

**When:** Significant choices made during maintenance

**Protocol:** `protocols/transparency.md`

**Instructions:** Document what was decided, why, and alternatives considered. Capture rationale before it's forgotten. Apply decision recording techniques.

**Inputs:**
- Decisions made (required)
- Context (optional)

**Default Output:** Decision record

---

### Verify No Regressions (→ KR1, KR2)

**Goal:** Confirm system still works correctly

**When:** Changes complete

**Protocols:** `protocols/software_quality.md`, `protocols/discipline.md`

**Instructions:** Run ALL tests systematically, spot-check behavior, verify documentation is accurate. Confirm system still works correctly. Apply quality dimensions and coverage tracking.

**Inputs:**
- Modified code (required)
- Test suite (required)
- Documentation (required)

**Default Output:** Verification report with pass/fail status

---

## Additional Notes and Terms

**Maintenance allocation:** IEEE recommends 15% of development time for refactoring and debt reduction.

**Maintenance vs Orienting:** Orienting assesses current state. Maintaining actively fixes issues and updates records.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Atlassian - Technical Debt Guide](https://www.atlassian.com/agile/software-development/technical-debt)
- [Write the Docs - Documentation Maintenance](https://www.writethedocs.org/guide/)
- [Google - Documentation Best Practices](https://google.github.io/styleguide/docguide/best_practices.html)
