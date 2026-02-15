# Assessment Approaches

Comprehensive assessment types for evaluating code quality during implementation. Skills can reference specific assessment types as needed.

---

## Overview

Assessment ensures code meets quality standards appropriate for the repository type. This guide defines **what to assess, how to measure it, and when to assess it**.

**Key principle:** Use **objective, tool-based measurements** with evidence instead of subjective self-assessment.

---

## Assessment Types

### 1. Modularity/Maintainability Assessment

**Purpose:** Ensure code is well-structured, maintainable, and not reinventing the wheel.

#### 1.1 Check for Existing Implementations

**Before creating new code, search for existing solutions:**

```bash
# Search for similar functionality
grep -r "function_name\|class_name" src/
rg "pattern" --type py

# Check imports to see what's available
grep -r "^from\|^import" src/ | sort | uniq
```

**Questions to ask:**
- Does this functionality already exist?
- Can I reuse an existing module?
- Is there a similar pattern I should follow?

**Evidence required:** List of searched patterns, what was found, decision rationale.

#### 1.2 The 8 Modularity Criteria

Comprehensive evaluation of code structure. See full details in sections below.

**Quick checklist:**
- [ ] **Cohesion** - Each module does ONE thing well (<5 public functions per module)
- [ ] **Coupling** - Minimal dependencies (<5 imports per file, no circular dependencies)
- [ ] **Coherence** - Logical module boundaries (no "utils" junk drawers)
- [ ] **Abstraction** - Appropriate layers (high-level doesn't manipulate low-level details)
- [ ] **Encapsulation** - Internal details hidden (private methods marked with `_`)
- [ ] **Testability** - Can test in isolation (easy to mock dependencies)
- [ ] **Reusability** - Generic interfaces (not tied to specific use case)
- [ ] **Complexity** - Simple code (complexity <10, functions <50 lines, nesting <4)

**Tools by language:**
- **Python:** `pylint`, `radon`, `pydeps`
- **JavaScript/TypeScript:** `eslint`, `madge`, `complexity-report`
- **Java:** `checkstyle`, `PMD`, `JDepend`
- **Go:** `gocyclo`, `golint`
- **Rust:** `cargo clippy`

**Detailed criteria with examples:**

##### Cohesion

**Definition:** Each module/class/function does ONE thing well.

**How to measure:**
```bash
# Python: Check for too many methods/attributes
pylint --disable=all --enable=R0904,R0902 src/

# Count public functions per module
find src/ -name "*.py" -exec grep -c "^def " {} \;
```

**Thresholds:**
- ✅ **Good:** <5 public functions, all related
- ⚠️ **Warning:** 5-10 public functions
- ❌ **Poor:** >10 public functions or unrelated functions

**Example:**
```python
# ❌ Low Cohesion
class UserManager:
    def create_user(self): pass
    def send_email(self): pass  # Doesn't belong
    def generate_report(self): pass  # Doesn't belong

# ✅ High Cohesion
class UserManager:
    def create_user(self): pass
    def update_user(self): pass
    def delete_user(self): pass
```

##### Coupling

**Definition:** Modules have minimal dependencies on other modules.

**How to measure:**
```bash
# Python: Count imports per file
find src/ -name "*.py" -exec grep -c "^import\|^from" {} \;

# Check for circular dependencies
pydeps src/ --show-deps
```

**Thresholds:**
- ✅ **Good:** <5 direct dependencies, no circular imports
- ⚠️ **Warning:** 5-10 dependencies
- ❌ **Poor:** >10 dependencies or circular imports

##### Complexity

**Definition:** Code is simple and understandable.

**How to measure:**
```bash
# Python: Cyclomatic complexity
radon cc src/ -a -nb
# Threshold: A-B rating (complexity 1-10)

# Check function length
# Functions should be <50 lines

# Check nesting depth
grep -E "    ( ){4,}" src/**/*.py  # 4+ levels
```

**Thresholds:**
- ✅ **Good:** Complexity <10, functions <30 lines, nesting <3
- ⚠️ **Warning:** Complexity 10-15, functions 30-50 lines
- ❌ **Poor:** Complexity >15, functions >50 lines, nesting >4

**For full details on all 8 criteria with tools and examples, see the legacy reference document.**

#### 1.3 Readability

**Code should be clear and understandable:**
- [ ] Variable names are descriptive
- [ ] Function names explain what they do
- [ ] Complex logic has explanatory comments
- [ ] No obscure abbreviations
- [ ] Consistent style throughout

**Evidence required:** Code review highlighting naming, comments, clarity.

---

### 2. Convention/Best Practices Assessment

**Purpose:** Ensure code follows project conventions and established patterns.

**What to check:**

#### 2.1 Follows CLAUDE.md Conventions

```bash
# Phase 0: Read project conventions
cat CLAUDE.md CONTRIBUTING.md README.md

# Document what conventions apply:
# - Naming patterns
# - Code structure
# - Standard tools/libraries
# - Testing practices
```

**Evidence required:**
- List conventions found in Phase 0
- Show how implementation follows each convention
- Note any deviations and why

#### 2.2 Uses Established Patterns

**Check for existing patterns in codebase:**
```bash
# How do other modules handle similar tasks?
grep -r "similar_pattern" src/

# What libraries/utilities are standard?
grep -r "^from utils\|^from lib" src/ | sort | uniq
```

**Questions:**
- Did I use existing utilities instead of creating new ones?
- Does my code structure match existing modules?
- Am I using the project's standard tools?

#### 2.3 Naming Conventions Match

**Verify naming matches project:**
- File naming (snake_case.py, camelCase.js, PascalCase.cs)
- Class naming
- Function naming
- Variable naming

**Evidence required:** Examples showing naming matches project style.

#### 2.4 Code Style Matches

**Run project linters:**
```bash
# Python
pylint src/ --rcfile=.pylintrc
black --check src/

# JavaScript
eslint src/
prettier --check src/

# Follow project's configured tools
```

**Evidence required:** Linter output showing all checks pass.

---

### 3. Accuracy/Correctness Assessment

**Purpose:** Verify the implementation works correctly and meets all requirements.

#### 3.1 Functional Correctness

**Does it work as specified?**
- [ ] All spec/PRD requirements implemented
- [ ] Edge cases handled (empty input, null, boundary values)
- [ ] Error cases handled (invalid input, failures, exceptions)
- [ ] Happy path works correctly

**Evidence required:** Test each requirement, show results.

#### 3.2 Test Coverage

**Tests pass and cover the code:**
```bash
# Python
pytest tests/ --cov=src --cov-report=term-missing
# Target: >85% for production, >50% for prototype

# JavaScript
npm test -- --coverage
# Target: >85% for production, >50% for prototype
```

**Requirements:**
- [ ] Unit tests for business logic
- [ ] Integration tests for critical flows
- [ ] Edge case tests
- [ ] Error handling tests
- [ ] Coverage meets repo threshold

**Evidence required:**
- Test output showing all tests pass
- Coverage report showing percentage
- List of what's tested

#### 3.3 No Regressions

**Existing tests still pass:**
```bash
# Run full test suite
pytest tests/
npm test

# All existing tests should pass
```

**Evidence required:** Test output showing no failures.

---

### 4. Non-Functional Requirements (NFRs)

**Note:** These vary by repo type. Customize based on CLAUDE.md requirements.

#### 4.1 Security Assessment

**For production repos only (skip for prototypes):**

**Check for vulnerabilities:**
```bash
# Python
bandit -r src/
safety check
pip-audit

# JavaScript
npm audit
snyk test

# General checks
grep -r "password\|secret\|api_key" src/  # No hard-coded secrets
```

**What to check:**
- [ ] No SQL injection vulnerabilities
- [ ] No XSS vulnerabilities
- [ ] No CSRF vulnerabilities
- [ ] Secrets not hard-coded
- [ ] Authentication/authorization correct
- [ ] Input validation on untrusted data
- [ ] Dependency vulnerabilities addressed

**Evidence required:** Security scan output showing no critical issues.

#### 4.2 Performance Assessment

**For production repos only (skip for prototypes):**

**Check performance requirements:**
```bash
# Response time benchmarks
# Use: ab, wrk, locust, or custom timing

# Profile code
python -m cProfile script.py

# Check database queries
# Use: EXPLAIN for SQL, query profilers
```

**What to check:**
- [ ] Response time requirements met (e.g., <100ms, <1s)
- [ ] No N+1 query problems
- [ ] No memory leaks
- [ ] Database queries optimized
- [ ] Large data sets handled efficiently

**Evidence required:** Benchmark results, profiler output showing requirements met.

#### 4.3 Other NFRs (As Needed)

**Based on repo requirements:**
- **Scalability** - Can handle expected load
- **Reliability** - Error handling, retries, fallbacks
- **Usability** - Clear error messages, good UX (for tools/CLIs)
- **Compatibility** - Works across required platforms/versions

---

### 5. Completeness Assessment

**Purpose:** Ensure all work is done and documented.

**Checklist:**
- [ ] All spec/plan requirements implemented
- [ ] No TODO comments left unaddressed
- [ ] No FIXME comments left unaddressed
- [ ] Code comments added where logic is complex
- [ ] README updated if needed
- [ ] CLAUDE.md updated with new conventions/patterns
- [ ] Integration points tested and working
- [ ] Documentation up to date

**Evidence required:** List of requirements vs what was implemented, show all items addressed.

---

## Customization by Repo Type

### Prototype Repos

**Focus on:**
- ✅ Functional correctness (does it work?)
- ✅ Basic readability
- ✅ Tests pass (>50% coverage acceptable)

**Skip:**
- ❌ Full modularity assessment (basic check only)
- ❌ Security scans
- ❌ Performance optimization
- ❌ Comprehensive documentation

### Production Repos

**All assessments required:**
- ✅ Full modularity assessment (all 8 criteria)
- ✅ Convention/best practices
- ✅ Correctness with high test coverage (≥85%)
- ✅ Security scans
- ✅ Performance requirements
- ✅ Complete documentation

### Library Repos

**Extra focus on:**
- ✅ API correctness (especially public APIs)
- ✅ Backward compatibility
- ✅ Semantic versioning compliance
- ✅ Comprehensive API documentation
- ✅ Very high test coverage (≥90% for public APIs)

**All production assessments plus:**
- ✅ Public API completeness
- ✅ Breaking change analysis
- ✅ Migration guides (if needed)

---

## Common Over-Evaluation Traps

**⚠️ LLMs tend to over-evaluate quality. Require evidence for all assessments.**

### Trap 1: "Looks Organized" ≠ Good Modularity

❌ **LLM might say:**
> "Modularity: Good - code is organized into multiple files"

✅ **Require evidence:**
> "Cohesion check: `pylint --enable=R0904` shows 0 violations. Each module has <5 public methods. Module purposes: auth.py (authentication), data.py (data access), api.py (endpoints). ✅ PASS"

### Trap 2: "No Errors" ≠ Low Coupling

❌ **LLM might say:**
> "Coupling: Low - code runs without import errors"

✅ **Require evidence:**
> "Coupling check: `pydeps --max-bacon=2` shows max depth 2. Import count per file: auth.py (3), api.py (4). No circular dependencies detected. ✅ PASS"

### Trap 3: "Functions Are Small" ≠ Low Complexity

❌ **LLM might say:**
> "Complexity: Low - functions are short"

✅ **Require evidence:**
> "Complexity check: `radon cc -a` shows average complexity 6.2 (Grade A). No functions >15 complexity. Nesting depth check finds 0 instances >4 levels. ✅ PASS"

### Trap 4: "Tests Exist" ≠ Good Coverage

❌ **LLM might say:**
> "Test coverage: Good - we have tests"

✅ **Require evidence:**
> "Coverage check: `pytest --cov` shows 87% coverage (meets 85% threshold). All critical paths tested. See coverage report: [output]. ✅ PASS"

---

## Assessment Template

Use this template when conducting assessments:

```markdown
# Quality Assessment: [Feature/Module Name]

**Date:** YYYY-MM-DD
**Repo Type:** [Prototype/Production/Library]

---

## 1. Modularity/Maintainability

### Existing Implementation Check
- Searched for: [patterns searched]
- Found: [existing code found or "none"]
- Decision: [reuse existing / create new because...]

### Cohesion
**Tool check:** `pylint --enable=R0904,R0902 src/module.py`
**Result:** [output]
**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [specific evidence]

### Coupling
**Tool check:** `grep -c "^import" src/module.py`
**Result:** [N] imports
**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [specific evidence]

### Complexity
**Tool check:** `radon cc src/module.py -a`
**Result:** Average complexity [N]
**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [specific evidence]

[Other criteria as needed...]

---

## 2. Conventions/Best Practices

**Conventions found in Phase 0:**
- [Convention 1]
- [Convention 2]

**How implementation follows conventions:**
- [Convention 1]: [how followed]
- [Convention 2]: [how followed]

**Linter check:** `pylint src/module.py`
**Result:** [score/output]
**Rating:** ✅ PASS / ❌ FAIL

---

## 3. Accuracy/Correctness

**Requirements implemented:**
- [Requirement 1]: ✅ Implemented
- [Requirement 2]: ✅ Implemented

**Test results:**
```bash
pytest tests/ --cov=src
```
**Result:** [all pass, N% coverage]
**Rating:** ✅ PASS / ❌ FAIL

---

## 4. NFRs (Production Only)

### Security
**Tool check:** `bandit -r src/`
**Result:** [output]
**Rating:** ✅ PASS / ❌ FAIL

### Performance
**Benchmark:** [requirements]
**Result:** [actual performance]
**Rating:** ✅ PASS / ❌ FAIL

---

## 5. Completeness

- [ ] All requirements implemented
- [ ] No unaddressed TODOs
- [ ] Documentation updated
- [ ] Integration points working

**Rating:** ✅ COMPLETE / ❌ INCOMPLETE

---

## Overall Assessment

**Summary:**
- Modularity: [✅ / ⚠️ / ❌]
- Conventions: [✅ / ❌]
- Correctness: [✅ / ❌]
- Security: [✅ / ❌ / N/A]
- Performance: [✅ / ❌ / N/A]
- Completeness: [✅ / ❌]

**Pass/Fail:** ✅ PASS / ❌ FAIL

**Required Actions:** [None / List of fixes needed]
```

---

## Summary

**Assessment is NOT subjective.** Always:
1. **Use tools** for objective measurement
2. **Require evidence** (tool output, test results, examples)
3. **Customize for repo type** (prototype vs production vs library)
4. **Be specific** about what passed/failed and why
5. **Show your work** - don't just say "looks good"

Skills should reference specific sections as needed:
- `[See references/assessment-approaches.md#modularity for the 8 criteria]`
- `[See references/assessment-approaches.md#conventions for convention checking]`
- `[See references/assessment-approaches.md#nfrs for security/performance]`
