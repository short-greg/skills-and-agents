**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: code-review-workflow
description: >
  Use when reviewing code for quality, conventions, and issues.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
  Triggers on: "review this code", "code review", "check code quality".
argument-hint: "[files or changes to review]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash, TodoWrite
---

# Code Review Workflow

**Goal:** Systematically review code for quality, conventions, and issues, producing actionable recommendations.

**Intent:** Ensure consistent, thorough reviews that check against project standards and provide clear remediation guidance. Prevent ad-hoc reviews that miss issues or lack actionable feedback.

**Scope:** Code quality assessment: understand conventions, review against standards, categorize issues by severity, produce structured report.

---

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Review approach is reasoned about before starting
2. All specified files reviewed — complete coverage of review scope
3. Issues are actionable — each issue has file:line reference and concrete recommendation
4. Issues are prioritized — categorized by severity (blocking, non-blocking, suggestion)
5. Quality dimensions assessed — correctness, clarity, reliability, completeness evaluated
6. Review concludes with decision — clear approval, needs changes, or blocked verdict

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `checklists.md` — preliminary checklist created before starting work, track which files/sections reviewed vs remain
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from next unreviewed file
3. Check all review categories systematically per `quality.md`
4. Every issue must have file:line reference
5. Every issue must have concrete recommendation
6. Severity must match impact (blocking = security/functionality/major violations, don't mix personal preference with blocking issues)
7. Per `goals_and_objectives.md` — review against defined quality criteria
8. Iterate up to 2 times if review needs revision

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Code to review (files, directory, or diff)

**Elicit if not provided:**
- Project conventions (read from CLAUDE.md, style guides, existing patterns)
- Review priorities (security-critical? performance-critical? API stability?)
- Output format preference (PR comments? Report? Issue tickets?)

**Optional:** Specific quality dimensions to focus on

## Postconditions

The resulting state after the skill is finished.

**Success:** All files reviewed, issues categorized, recommendations provided, decision made.

**Failure:** Files don't exist or aren't readable, conventions cannot be determined, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason About Review
2. Understand Context
3. Review Against Standards
4. Categorize Issues
5. Generate Report

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Review (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Review scope** — What files? Full review or focused?
2. **Project context** — What conventions apply? What matters most?
3. **Priority categories** — Security-critical? Performance-critical? API stability?
4. **Output format** — PR comments? Report? Issue tickets?

Output your reasoning.

### Understand Context (→ KR1, KR5)

Use `orient` primitive. Understand project conventions, coding standards, test conventions.

Read: CLAUDE.md, style guides, existing patterns.

### Review Against Standards (→ KR2, KR5)

Use `validate` primitive. Check code against each category systematically.

Per `quality.md` — apply quality dimensions:

- **Code Standards:** naming, imports, types, docstrings, error handling
- **Design Principles:** single responsibility, DRY, modularity, coherence
- **Maintainability:** self-documenting, comments for complex logic, edge cases
- **Testing:** tests exist, happy path, error cases, edge cases
- **Security:** injection, input validation, secrets, sensitive data

### Categorize Issues (→ KR3, KR4)

Use `critique` primitive. Classify by severity:

- **Blocking:** Security, data integrity, functional bugs, breaking changes
- **Non-blocking:** Code quality, minor violations, missing coverage
- **Suggestion:** Style preference, optimization, alternative approach

### Generate Report (→ KR6)

Output structured report:

```markdown
## Code Review Report

**Files Reviewed:** [list]
**Status:** Approved / Needs Changes / Blocked

### Summary
[2-3 sentences on overall quality]

### Blocking Issues (must fix)
| File:Line | Category | Issue | Recommendation |

### Non-Blocking Issues (should fix)
| File:Line | Category | Issue | Recommendation |

### Suggestions
- [item]

### Approval Decision
- [ ] Approved - Ready to merge
- [ ] Needs Changes - Address blocking issues
- [ ] Blocked - Major issues require redesign
```

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand project conventions, coding standards, test conventions
- `validate` — Check code against standards systematically
- `critique` — Identify issues, categorize by severity, suggest improvements

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

**Behavior Preservation:**

Behavior is preserved when:
- Public API contracts unchanged
- All existing tests pass
- No observable changes from client perspective

**Customization Points:**

- **Prototype:** Focus on blocking issues only, skip test coverage requirements
- **Production:** Full review against all categories, comprehensive security review
- **Library:** Extra focus on public API documentation, backward compatibility, changelog
