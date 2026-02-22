---
name: code-review-workflow
description: >
  Use when reviewing code for quality, conventions, and issues.
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

## Key Results

1. Review approach is reasoned about before starting
2. All specified files reviewed — complete coverage of review scope
3. Issues are actionable — each issue has file:line reference and concrete recommendation
4. Issues are prioritized — categorized by severity (blocking, non-blocking, suggestion)
5. Quality dimensions assessed — correctness, clarity, reliability, completeness evaluated
6. Review concludes with decision — clear approval, needs changes, or blocked verdict

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which files/sections have been reviewed vs remain
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist after reasoning about the review. Update it dynamically.
- `reasoning.md` — Reason about review approach before starting. Verify thoroughness after.
- `goals_and_objectives.md` — Review against defined quality criteria.
- `quality.md` — Evaluate against quality dimensions: correctness, clarity, maintainability.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to review. If you do not understand a primitive, read it before using it.

- `orient` — Understand project conventions, coding standards, test conventions.
- `validate` — Check code against standards systematically.
- `critique` — Identify issues, categorize by severity, suggest improvements.

---

## Constraints

- Check all review categories systematically
- Every issue must have file:line reference
- Every issue must have recommendation
- Severity must match impact (blocking = security/functionality/major violations)
- Don't mix personal preference with blocking issues

---

## Tasks

Execute these tasks to achieve the key results. Select and sequence based on your reasoning about the review.

### Reason About Review

Per `reasoning.md` — before beginning, reason about:

1. **Review scope** — What files? Full review or focused?
2. **Project context** — What conventions apply? What matters most?
3. **Priority categories** — Security-critical? Performance-critical? API stability?
4. **Output format** — PR comments? Report? Issue tickets?

Output your reasoning.

### Understand Context

Use `orient` primitive. Understand project conventions, coding standards, test conventions.

Read: CLAUDE.md, style guides, existing patterns.

### Review Against Standards

Use `validate` primitive. Check code against each category systematically.

Per `quality.md` — apply quality dimensions:

- **Code Standards:** naming, imports, types, docstrings, error handling
- **Design Principles:** single responsibility, DRY, modularity, coherence
- **Maintainability:** self-documenting, comments for complex logic, edge cases
- **Testing:** tests exist, happy path, error cases, edge cases
- **Security:** injection, input validation, secrets, sensitive data

### Categorize Issues

Use `critique` primitive. Classify by severity:

- **Blocking:** Security, data integrity, functional bugs, breaking changes
- **Non-blocking:** Code quality, minor violations, missing coverage
- **Suggestion:** Style preference, optimization, alternative approach

### Generate Report

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

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist after reasoning about the review, based on what you learned
- Track which files have been reviewed
- Mark items complete immediately after finishing each category
- Report progress after each completed category

---

## Preconditions

**Must be provided:** Code to review (files, directory, or diff)

**Self-satisfiable:** Project conventions (read from docs and existing code)

---

## Postconditions

**Success:** All files reviewed, issues categorized, recommendations provided, decision made.

**Failure:** Files don't exist or aren't readable, conventions cannot be determined (ask user).

---

## Recovery

Per `recovery.md` — check for existing trace on startup.

**Task-specific notes:**
- Track which files reviewed
- Resume from next unreviewed file
- Report generation is idempotent — regenerate from categorized issues

---

## Customization Points

**Prototype:** Focus on blocking issues only, skip test coverage requirements.

**Production:** Full review against all categories, comprehensive security review.

**Library:** Extra focus on public API documentation, backward compatibility, changelog.
