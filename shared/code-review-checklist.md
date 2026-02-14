# Code Review Checklist

Universal code review standards used by implementation skills.

---

## Purpose

Provide a consistent checklist for code review across all implementation skills (feature-impl, bugfix-impl, refactor-impl).

---

## Code Quality Checklist

### Code Standards

- [ ] Follows project naming conventions (PascalCase classes, snake_case functions, etc.)
- [ ] Proper import ordering (stdlib, third-party, framework, local)
- [ ] Uses type hints consistently
- [ ] Docstrings for public APIs
- [ ] No hardcoded values (use config/parameters)
- [ ] Appropriate error handling (no silent failures)
- [ ] No security vulnerabilities (injection, XSS, etc.)

### Design Principles

- [ ] **Single Responsibility**: Each class/function does one thing
- [ ] **DRY**: No duplicated code
- [ ] **Modular**: Components are loosely coupled
- [ ] **Coherent**: Related functionality is together
- [ ] **Composable**: Components can be reused
- [ ] **YAGNI**: No over-engineering

### Maintainability

- [ ] Code is self-documenting (clear names)
- [ ] Complex logic has explanatory comments
- [ ] Edge cases are handled
- [ ] Logging at appropriate levels
- [ ] Error messages are helpful

### Testing

- [ ] All tests pass
- [ ] Tests cover happy path
- [ ] Tests cover error cases
- [ ] Tests cover edge cases
- [ ] No test duplication
- [ ] Test names describe what they test

---

## Design Verification

Verify implementation matches approved design:

- [ ] File structure matches plan
- [ ] Classes/functions match specifications
- [ ] Dependencies are as designed
- [ ] Data flow follows the plan
- [ ] Interfaces match contracts

---

## Repository Conventions

Ensure consistency with codebase:

- [ ] Same patterns as similar features
- [ ] Consistent error handling approach
- [ ] Aligned with framework usage patterns
- [ ] Matches project style guide
- [ ] Follows existing naming conventions

---

## Refactoring Opportunities

When reviewing, identify improvements:

### Required (blocking)

Issues that must be fixed before approval:
- Security vulnerabilities
- Failing tests
- Missing error handling for critical paths
- Code that doesn't meet acceptance criteria

### Recommended (non-blocking)

Improvements to consider:
- Extracting duplicated code
- Simplifying complex conditionals
- Better naming
- Additional test coverage

---

## Review Output Format

```markdown
## Code Review: [Feature/Bug/Refactor Name]

**Reviewed:** YYYY-MM-DD
**Status:** Approved / Needs Changes / Blocked

### Code Standards
- [x] Naming conventions followed
- [x] Type hints consistent
- [ ] Missing docstring on `process_input()` (line 45)

### Design Principles
- [x] Single responsibility maintained
- [x] No duplication
- [ ] Consider extracting validation to separate class

### Testing
- [x] All tests pass
- [x] Happy path covered
- [ ] Missing edge case: empty input handling

### Required Changes
1. Add docstring to `process_input()` (line 45)
2. Add test for empty input case

### Recommendations
1. Extract validation logic to ValidationService class
2. Consider adding debug logging in transform step

### Approval Decision
- [ ] Approved - Ready to merge
- [x] Needs Changes - Address required changes above
- [ ] Blocked - Major issues require redesign
```

---

## Usage in Setup Guides

Each implementation skill setup guide should reference this document:

```markdown
## Phase N: Code Review

Follow the code review checklist from [shared/code-review-checklist.md](../shared/code-review-checklist.md).

**Suite-specific review criteria:**
- [Any additional criteria specific to this skill]
```
