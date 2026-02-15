# Checklist Management

How to create and manage checklists in skills with conditionals, loop-backs, and progress tracking.

---

## Overview

Checklists help skills track progress and ensure completeness. This guide defines the standard format and rules for managing checklists across all skills.

---

## Standard Checklist Format

**Format:** Numbered items with conditions in parentheses

```markdown
- [ ] 1. (If production repo) Run security scans (bandit, safety)
- [ ] 2. Always: Run tests and check coverage ≥85%
- [ ] 3. (If coverage <85%) Fix tests and return to step 2
- [ ] 4. Always: Update ${SPEC_DIR}/plan.md with progress
```

**Key elements:**
- **Numbered items:** Makes referencing easy ("return to step 2")
- **Conditions in parentheses:** Clear when item applies
- **Always prefix:** Indicates unconditional items
- **Descriptive actions:** Clear what to do

---

## Conditional Items

### Basic Conditionals

Show condition in parentheses at start of item:

```markdown
- [ ] 1. (If production repo) Run security scans
- [ ] 2. (If library) Check semantic versioning compliance
- [ ] 3. (If breaking changes) Write migration guide
```

**When condition is met:** Execute the item
**When condition is NOT met:** Skip the item (no need to mark SKIP)

### Common Condition Types

#### Repository Type Conditions

```markdown
- [ ] (If production repo) Run full security scan
- [ ] (If prototype repo) Basic functionality check only
- [ ] (If library) Verify backward compatibility
- [ ] (If tool/CLI) Test error messages and help text
```

#### State/Result Conditions

```markdown
- [ ] (If tests fail) Fix failures and return to step N
- [ ] (If coverage <85%) Add more tests
- [ ] (If vulnerabilities found) Address security issues
- [ ] (If complexity >10) Refactor for simplicity
```

#### Change Type Conditions

```markdown
- [ ] (If breaking changes) Update CHANGELOG with migration guide
- [ ] (If new public API) Add API documentation
- [ ] (If database changes) Create migration script
```

---

## Always Items

Mark items that must execute regardless of conditions:

```markdown
- [ ] 1. Always: Run core tests
- [ ] 2. Always: Verify no syntax errors
- [ ] 3. Always: Update spec/plan with progress
```

**Why use "Always":** Makes it explicit that this item is unconditional, even in a list with many conditionals.

---

## Loop-Back References

Allow retry loops without duplicating checklist items:

```markdown
- [ ] 1. Run tests
- [ ] 2. Check coverage ≥85%
- [ ] 3. (If coverage <85%) Add tests and return to step 1
- [ ] 4. Run security scan
- [ ] 5. (If vulnerabilities found) Fix issues and return to step 4
```

**How it works:**
- Step 3 conditionally unchecks and returns to step 1
- Avoids duplicating "Run tests" multiple times
- Clear flow: test → check → fix if needed → repeat

**When to use:**
- Tests fail → fix and rerun
- Coverage too low → add tests and retest
- Quality check fails → improve and recheck

---

## Required Items

### Every Checklist MUST Include

**Progress update item:**

```markdown
- [ ] N. Always: Update ${SPEC_DIR}/[plan-name].md with:
  - Progress made in this phase
  - Design changes or pivots
  - Challenges encountered
  - Solutions implemented
  - Next steps
```

**Why:** Creates audit trail, documents decisions, helps with handover.

**See:** [spec-maintenance.md](spec-maintenance.md) for what to document.

---

## Skip Transparency

### Automatic Transparency

**Conditions make skipping obvious:**

```markdown
# In a production repo:
- [ ] 1. Run tests ✓
- [ ] 2. (If production repo) Security scans ✓  # Executed
- [ ] 3. Update spec ✓

# In a prototype repo:
- [ ] 1. Run tests ✓
- [ ] 2. (If production repo) Security scans  # Clearly doesn't apply
- [ ] 3. Update spec ✓
```

**No need to explicitly mark "SKIP" or add reasoning** - the condition itself documents why it was skipped.

### When to Add Notes

**Only add explicit notes when:**
- Condition is met but you chose to skip anyway (explain why)
- Unexpected situation requires clarification

**Example:**
```markdown
- [ ] (If production repo) Run security scans
      Note: Skipped because security scan is broken (issue #123), will run manually
```

---

## Examples by Repo Type

### Prototype Repo Checklist

```markdown
## Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests (coverage >50%)
- [ ] 2. Verify basic functionality works
- [ ] 3. Check code is readable
- [ ] 4. (Skip for prototype) Security scans
- [ ] 5. (Skip for prototype) Full modularity assessment
- [ ] 6. (Skip for prototype) Performance optimization
- [ ] 7. Always: Update ${SPEC_DIR}/plan.md with progress
```

**Notes:**
- Lower coverage threshold (50% vs 85%)
- Skip security, modularity, performance (marked clearly)
- Still requires progress update

### Production Repo Checklist

```markdown
## Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests
- [ ] 2. Check coverage ≥85%
- [ ] 3. (If coverage <85%) Add tests and return to step 1
- [ ] 4. Run security scans (bandit, safety, npm audit)
- [ ] 5. (If vulnerabilities found) Fix issues and return to step 4
- [ ] 6. Run modularity assessment (8 criteria)
- [ ] 7. (If modularity issues) Refactor and return to step 6
- [ ] 8. Check performance requirements (<100ms response time)
- [ ] 9. (If performance issues) Optimize and return to step 8
- [ ] 10. Always: Update ${SPEC_DIR}/plan.md with progress, challenges, solutions
```

**Notes:**
- High coverage threshold (85%)
- All quality gates included
- Loop-backs for fixing issues
- Comprehensive progress update

### Library Repo Checklist

```markdown
## Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests
- [ ] 2. Check coverage ≥90% (especially public APIs)
- [ ] 3. (If coverage <90%) Add tests and return to step 1
- [ ] 4. Verify all public APIs documented
- [ ] 5. (If documentation missing) Add docs and return to step 4
- [ ] 6. Check semantic versioning compliance
- [ ] 7. (If breaking changes) Update CHANGELOG with migration guide
- [ ] 8. Run backward compatibility tests
- [ ] 9. (If compatibility broken) Fix and return to step 8
- [ ] 10. Always: Update ${SPEC_DIR}/plan.md with progress
```

**Notes:**
- Very high coverage (90%, focus on public APIs)
- API documentation required
- Semver and compatibility checks
- Migration guides for breaking changes

---

## Checklist Best Practices

### Keep Items Focused

✅ **Good:**
```markdown
- [ ] 1. Run unit tests
- [ ] 2. Run integration tests
- [ ] 3. Check coverage ≥85%
```

❌ **Bad:**
```markdown
- [ ] 1. Run all tests and check coverage and verify everything passes
```

### Use Specific Conditions

✅ **Good:**
```markdown
- [ ] 1. (If coverage <85%) Add tests for untested code
```

❌ **Bad:**
```markdown
- [ ] 1. (If needed) Maybe add some tests
```

### Make Actions Clear

✅ **Good:**
```markdown
- [ ] 1. Run `pytest tests/ --cov=src` and verify ≥85% coverage
```

❌ **Bad:**
```markdown
- [ ] 1. Do testing stuff
```

### Order Logically

**Good order:**
1. Prerequisites first
2. Main work
3. Verification
4. Fixes (with loop-backs)
5. Documentation/progress update last

**Example:**
```markdown
- [ ] 1. Read ${SPEC_DIR}/plan.md  # Prerequisite
- [ ] 2. Implement feature  # Main work
- [ ] 3. Run tests  # Verification
- [ ] 4. (If tests fail) Fix and return to step 3  # Fix loop
- [ ] 5. Always: Update plan.md  # Documentation
```

---

## Advanced Patterns

### Parallel Conditions

When multiple conditions could apply:

```markdown
- [ ] 1. (If production repo) Run full security scan
- [ ] 2. (If library) Check API backward compatibility
- [ ] 3. (If tool/CLI) Verify error messages are user-friendly
```

All applicable items execute independently.

### Nested Conditions

When conditions depend on each other:

```markdown
- [ ] 1. (If production repo) Run security scan
- [ ] 2. (If production repo AND vulnerabilities found) Create security fix plan
```

Second item only runs if both conditions true.

### Progressive Quality

Start with basics, add more if needed:

```markdown
- [ ] 1. Basic functionality works
- [ ] 2. Tests pass
- [ ] 3. (If production repo) Run full quality suite
- [ ] 4. (If production repo AND any quality issues) Address issues
```

Prototype stops at step 2, production continues.

---

## Integration with Progress Tracking

### Update Checklist State

Mark items as you complete them:

```markdown
**Initial state:**
- [ ] 1. Run tests
- [ ] 2. Check coverage
- [ ] 3. Update plan

**After step 1:**
- [x] 1. Run tests  ← Completed
- [ ] 2. Check coverage  ← In progress
- [ ] 3. Update plan

**All done:**
- [x] 1. Run tests
- [x] 2. Check coverage
- [x] 3. Update plan
```

### Report Progress

**After each phase:**

```markdown
**Phase 5 Progress:**

Checklist status: 7/10 items complete

Completed:
- ✅ Run tests (all pass)
- ✅ Coverage check (87%, meets threshold)
- ✅ Security scan (no vulnerabilities)

In progress:
- 🔄 Modularity assessment (cohesion ✅, checking coupling...)

Remaining:
- ⏸️ Performance check
- ⏸️ Update documentation
- ⏸️ Update plan
```

---

## Common Mistakes to Avoid

### ❌ Mistake 1: Vague Conditions

**Bad:** `(If needed)`
**Good:** `(If coverage <85%)`

### ❌ Mistake 2: No Loop-Backs

**Bad:**
```markdown
- [ ] 1. Run tests
- [ ] 2. Fix tests
- [ ] 3. Run tests again
- [ ] 4. Fix tests again
```

**Good:**
```markdown
- [ ] 1. Run tests
- [ ] 2. (If tests fail) Fix and return to step 1
```

### ❌ Mistake 3: Missing Always Items

**Bad:**
```markdown
- [ ] 1. (If production) Full tests
- [ ] 2. (If prototype) Basic tests
# What if it's neither? No tests run!
```

**Good:**
```markdown
- [ ] 1. Always: Run basic tests
- [ ] 2. (If production) Run additional security tests
```

### ❌ Mistake 4: Implementation Steps in High-Level Checklist

**Bad (in PRD checklist):**
```markdown
- [ ] Create database table
- [ ] Write API endpoint
- [ ] Add frontend component
```

**Good (in PRD checklist):**
```markdown
- [ ] All requirements defined
- [ ] Success criteria measurable
- [ ] Stakeholder approval obtained
```

**Note:** Implementation steps go in implementation phase checklists, not planning phase checklists.

---

## Summary

**Standard format:**
- Numbered items
- Conditions in parentheses
- "Always:" prefix for unconditional items
- Loop-backs for retries

**Key rules:**
- Every checklist MUST include progress update item
- Conditions make skipping transparent (no need to mark SKIP)
- Use loop-backs instead of duplicating items
- Make conditions specific and measurable

**Customize by repo type:**
- Prototype: Simpler, fewer quality gates
- Production: Comprehensive, all quality gates
- Library: Extra API/compatibility checks

Skills should reference this document:
- `[See references/checklist-management.md for checklist patterns]`
- `[See references/checklist-management.md#conditionals for if/else patterns]`
