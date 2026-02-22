---
name: code-review-workflow
description: >
  Use when reviewing code for quality, conventions, and issues.
  Triggers on: "review this code", "code review", "check code quality".
argument-hint: "[files or changes to review]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Bash
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Code Review Workflow

**Goal:** Systematically review code for quality, conventions, security, and design issues, producing a structured report with actionable recommendations.

**Intent:** Ensure consistent, thorough code reviews that check against project standards, identify issues by severity, and provide clear remediation guidance. Prevent ad-hoc reviews that miss important issues or lack actionable feedback.

**Scope:** Code quality assessment. Includes: understanding project conventions, reviewing code against standards (code quality, design principles, maintainability, testing, security), categorizing issues by severity, and producing a structured report. Does NOT include: implementing fixes (that's a separate task) or architectural decisions (use design primitive).

---

## Workflow Type

**Type:** Deterministic

**Style:** Imperative

**Assumptions:**
- Code to review exists and is readable
- Project conventions can be determined from docs or existing code
- Review criteria are well-defined (standards, design principles, security)

**Note:** While deterministic in structure, issue severity judgments involve some subjectivity.

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand project conventions, existing patterns, and context for the code being reviewed

**Inputs:** Files or changes to review

**Outputs:**
- Project conventions identified (naming, structure, patterns)
- Coding standards identified (style guide, linter rules)
- Test conventions identified (framework, coverage expectations)
- Context for what the code does and why

**Self-satisfiable:** Reads project documentation (CLAUDE.md, .cursorrules, etc.) and existing code patterns

### Step 2: Review Against Standards

**Primitive:** validate

**Purpose:** Check code against each review category systematically

**Inputs:**
- Code to review
- Project conventions from Step 1

**Outputs:**
- Issues found per category
- Evidence for each issue (file:line references)
- Initial severity assessment

**Review categories:**

**Code Standards:**
- Follows project naming conventions
- Proper import ordering
- Type hints used consistently (if applicable)
- Docstrings for public APIs
- No hardcoded values (magic numbers/strings)
- Appropriate error handling

**Design Principles:**
- Single Responsibility: Each class/function does one thing
- DRY: No duplicated code
- Modular: Components are loosely coupled
- Coherent: Related functionality is together
- Composable: Components can be reused

**Maintainability:**
- Code is self-documenting
- Complex logic has comments
- Edge cases are handled
- Logging at appropriate levels

**Testing:**
- Tests exist for new/changed code
- Tests cover happy path
- Tests cover error cases
- Tests cover edge cases

**Security:**
- No injection vulnerabilities (SQL, command, XSS)
- Input validation present
- Sensitive data handled properly
- No hardcoded secrets

### Step 3: Categorize Issues

**Primitive:** critique

**Purpose:** Classify each issue by severity and provide actionable recommendations

**Inputs:**
- Issues from Step 2
- Project context from Step 1

**Outputs:**
- Issues categorized by severity:
  - **Blocking:** Must fix before approval (security vulnerabilities, broken functionality, major violations)
  - **Non-blocking:** Should fix but not required (code quality, minor violations, improvements)
  - **Suggestion:** Nice to have (style preferences, optimization opportunities)
- Actionable recommendation for each issue

**Categorization criteria:**

| Severity | Criteria |
|----------|----------|
| Blocking | Security vulnerability, data integrity risk, functional bug, breaking API contract |
| Non-blocking | Code quality issue, minor convention violation, missing test coverage |
| Suggestion | Style preference, potential optimization, alternative approach |

### Step 4: Generate Report

**Primitive:** implement

**Purpose:** Produce structured review report

**Inputs:**
- Categorized issues from Step 3
- Files reviewed

**Outputs:**
- Structured report following output format
- Approval decision based on issues found

**Report structure:**

```markdown
## Code Review Report

**Files Reviewed:** [list of files]
**Date:** YYYY-MM-DD
**Status:** Approved / Needs Changes / Blocked

### Summary

[2-3 sentence summary of overall code quality]

### Issues Found

#### Blocking Issues (must fix)

| # | File:Line | Category | Issue | Recommendation |
|---|-----------|----------|-------|----------------|
| 1 | path.py:45 | Security | SQL injection risk | Use parameterized queries |

#### Non-Blocking Issues (should fix)

| # | File:Line | Category | Issue | Recommendation |
|---|-----------|----------|-------|----------------|
| 1 | path.py:23 | Naming | Unclear variable name | Rename `x` to `user_count` |

#### Suggestions (nice to have)

- Consider extracting validation logic to separate function
- Could use list comprehension for cleaner syntax

### Approval Decision

- [ ] **Approved** - Ready to merge
- [x] **Needs Changes** - Address blocking issues
- [ ] **Blocked** - Major issues require redesign

### Next Steps

1. [Specific action items]
```

---

## Preconditions

**Must be provided:**
- code to review: files, directory, or diff

**Self-satisfiable:**
- project conventions: read from project docs and existing code
- review standards: use default categories (code, design, maintainability, testing, security)

---

## Postconditions

**Success:**
- All specified files reviewed
- Issues categorized by severity
- Recommendations provided for each issue
- Approval decision made

**Failure (blocked):**
- Files to review don't exist or aren't readable
- Unable to determine project conventions (ask user)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify which files have been reviewed
   - Resume from next unreviewed file
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1 (Orient):** Safe to re-run
- **Step 2 (Review):** If partial, read issues found so far and continue
- **Step 3 (Categorize):** If partial, read categorizations so far and continue
- **Step 4 (Report):** Safe to re-run; regenerate from categorized issues

---

## On Validation Failure

This workflow doesn't have validation failures in the typical sense—it produces a report of what it finds. However:

**If unable to review:**
1. Identify blocking issue (file not found, no permissions, etc.)
2. Report partial results if any files were reviewed
3. Note which files couldn't be reviewed and why

---

## Quality Gates

| Step | Gate | Purpose |
|------|------|---------|
| Step 2 | All categories checked | Ensure comprehensive review |
| Step 4 | Report complete | Verify all issues documented |

**User approval gates:** None required (review produces report, user decides action)

---

## Customization Points

When adapting this workflow for your project:

### Repository Type

**Prototype:**
- Focus on blocking issues only
- Skip test coverage requirements
- Minimal security checks
- Faster, lighter review

**Production:**
- Full review against all categories
- Require test coverage for changes
- Comprehensive security review
- All severity levels reported

**Library:**
- Extra focus on public API documentation
- Backward compatibility checks
- API design review
- Changelog considerations

### Review Categories

- Add framework-specific checks (Django, React, etc.)
- Adjust severity levels per project policy
- Add domain-specific criteria (compliance, accessibility, etc.)

### Output Format

- Integrate with PR comments (GitHub, GitLab)
- Generate issue tickets for blocking items
- Custom report sections

---

## Example Usage

**User:** "Review the changes in src/auth/"

**Workflow execution:**

1. **Orient** — Read project conventions, identify auth module patterns
2. **Review** — Check each file against categories:
   - `login.py`: Found SQL injection risk (Security), missing input validation (Security)
   - `session.py`: Hardcoded timeout value (Code), missing docstring (Maintainability)
   - `test_login.py`: Good coverage, tests pass
3. **Categorize:**
   - Blocking: SQL injection (login.py:45)
   - Non-blocking: Hardcoded timeout (session.py:23), missing docstring (session.py:1)
   - Suggestion: Consider input validation middleware
4. **Report:** Generate structured report with "Needs Changes" status

**Outcome:** Clear report with actionable items, user knows exactly what to fix.

---

## Best Practices

- **Be specific** — File:line references for every issue
- **Be actionable** — Every issue has a recommendation
- **Be fair** — Categorize by impact, not personal preference
- **Be constructive** — Focus on improvement, not criticism
- **Be complete** — Check all categories, don't shortcut

---

## Common Anti-Patterns

**Anti-Pattern 1: Vague feedback**
- ❌ "This code could be better"
- ✅ "login.py:45 - Use parameterized queries to prevent SQL injection"

**Anti-Pattern 2: Over-blocking**
- ❌ Marking style preferences as blocking
- ✅ Reserve blocking for security, functionality, major violations

**Anti-Pattern 3: Missing recommendations**
- ❌ "This is wrong"
- ✅ "This is wrong because X, fix by doing Y"

**Anti-Pattern 4: Incomplete review**
- ❌ "I checked the main file, looks fine"
- ✅ Review all files, all categories systematically

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [project_quality.md](../protocols/project_quality.md) — Quality assessment dimensions
  - [modularity.md](../protocols/modularity.md) — Design principle evaluation

---

## Related Primitives

This workflow composes these primitives:

1. [orient](../primitives/orient.md) — Understand project conventions
2. [validate](../primitives/validate.md) — Check code against standards
3. [critique](../primitives/critique.md) — Categorize and assess issues
4. [implement](../primitives/implement.md) — Generate report

---

## Related Workflows

- [feature_workflow](feature_workflow.md) — May trigger code review before completion
- [refactor_workflow](refactor_workflow.md) — May use code review to verify quality improvement
