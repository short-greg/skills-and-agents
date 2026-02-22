---
name: code-review-workflow
description: >
  Use when reviewing code for quality, conventions, and issues.
  Triggers on: "review this code", "code review", "check code quality".
argument-hint: "[files or changes to review]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash, TodoWrite
protocols:
  - tracking
  - recovery
  - checklist_management
  - reasoning_patterns
  - goals_and_objectives
  - project_quality
---

# Code Review Workflow

**Goal:** Systematically review code for quality, conventions, and issues, producing actionable recommendations.

**Intent:** Ensure consistent, thorough reviews that check against project standards and provide clear remediation guidance. Prevent ad-hoc reviews that miss issues or lack actionable feedback.

**Scope:** Code quality assessment: understand conventions, review against standards, categorize issues by severity, produce structured report.

---

## Workflow Type

**Type:** Deterministic

**Style:** Hybrid (declarative goals with imperative category checklist)

---

## Key Results

Per `goals_and_objectives` protocol — outcome-oriented results:

**Required:**
1. **All specified files reviewed** — complete coverage of review scope
2. **Issues are actionable** — each issue has file:line reference and concrete recommendation
3. **Issues are prioritized** — categorized by severity (blocking, non-blocking, suggestion) per impact
4. **Review concludes with decision** — clear approval, needs changes, or blocked verdict

**Conditional:**
5. **Quality dimensions assessed** — correctness, clarity, reliability, completeness evaluated (per `project_quality`)

---

## Available Primitives

`orient`, `validate`, `critique`

---

## Constraints

- Check all review categories systematically
- Every issue must have file:line reference
- Every issue must have recommendation
- Severity must match impact (blocking = security/functionality/major violations)
- Don't mix personal preference with blocking issues

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Review scope** — What files? Full review or focused?
2. **Project context** — What conventions apply? What matters most?
3. **Priority categories** — Security-critical? Performance-critical? API stability?
4. **Output format** — PR comments? Report? Issue tickets?

Output reasoning before proceeding.

---

## Progress Tracking (Required)

Per `checklist_management` protocol — create and maintain a checklist throughout execution.

**On workflow start, create this checklist:**

```markdown
## Code Review Progress

- [ ] 1. Understand context (orient)
- [ ] 2. Review against standards (validate)
  - [ ] 2a. Code standards
  - [ ] 2b. Design principles
  - [ ] 2c. Maintainability
  - [ ] 2d. Testing
  - [ ] 2e. Security
- [ ] 3. Categorize issues (critique)
- [ ] 4. Generate report
```

**Rules:**
- Display checklist at start of workflow
- Mark items `[x]` immediately after completing each category
- Track which files have been reviewed
- Report progress after each completed category: "✅ [category] — [N issues found]"

---

## Execution

Execute by selecting and sequencing primitives to achieve key results.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand project conventions, coding standards, test conventions.

Read: CLAUDE.md, style guides, existing patterns.

### Phase 2: Review Against Standards

**Primitive:** `validate` — See `primitives/validate.md`

Check code against each category systematically.

Per `project_quality` protocol — apply quality dimensions:

**Code Standards:** naming, imports, types, docstrings, error handling
**Design Principles:** single responsibility, DRY, modularity, coherence
**Maintainability:** self-documenting, comments for complex logic, edge cases
**Testing:** tests exist, happy path, error cases, edge cases
**Security:** injection, input validation, secrets, sensitive data

### Phase 3: Categorize Issues

**Primitive:** `critique` — See `primitives/critique.md`

Classify by severity:
- **Blocking:** Security, data integrity, functional bugs, breaking changes
- **Non-blocking:** Code quality, minor violations, missing coverage
- **Suggestion:** Style preference, optimization, alternative approach

### Phase 4: Generate Report

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

## Preconditions

**Must be provided:** Code to review (files, directory, or diff)

**Self-satisfiable:** Project conventions (read from docs and existing code)

---

## Postconditions

**Success:** All files reviewed, issues categorized, recommendations provided, decision made.

**Failure:** Files don't exist or aren't readable, conventions cannot be determined (ask user).

---

## Recovery

Per `recovery` protocol — check for existing trace on startup.

**Step-specific notes:**
- Track which files reviewed
- Resume from next unreviewed file
- Report generation is idempotent — regenerate from categorized issues

---

## Customization Points

**Prototype:** Focus on blocking issues only, skip test coverage requirements.

**Production:** Full review against all categories, comprehensive security review.

**Library:** Extra focus on public API documentation, backward compatibility, changelog.
