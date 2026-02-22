---
name: test-strategy-workflow
description: >
  Use when designing test strategies for features or code changes.
  Triggers on: "design tests for", "test strategy", "what tests do I need".
argument-hint: "[feature or code to design tests for]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Test Strategy Workflow

**Goal:** Design comprehensive test strategies applying test pyramid principles, identifying test cases, and optionally generating test code structures.

**Intent:** Ensure features have thoughtful test coverage before implementation by systematically analyzing testability, applying test pyramid principles, identifying edge cases, and producing either a strategy document or test code skeleton. Prevent ad-hoc testing that misses edge cases or has poor coverage distribution.

**Scope:** Test design and planning. Includes: understanding code to test, analyzing testability (units, integrations, boundaries), designing test strategy (pyramid distribution), identifying test cases (happy path, errors, edge cases), and producing output (strategy doc or test code). Does NOT include: running tests or fixing failing tests (those are implementation tasks).

---

## Workflow Type

**Type:** Deterministic

**Style:** Hybrid (imperative analysis, declarative output options)

**Assumptions:**
- Feature or code to test exists or is well-specified
- Test framework is known or can be determined
- Test pyramid principles apply (unit > integration > e2e)

**Output options:**
- Strategy document (for planning)
- Generated test code (for implementation)

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand code/feature to test and project test conventions

**Inputs:** Feature specification or code to design tests for

**Outputs:**
- Code/feature understood (purpose, components, dependencies)
- Test conventions identified (framework, patterns, file locations)
- Existing test patterns identified (fixtures, mocks, utilities)

**Self-satisfiable:** Reads feature spec, code, and existing tests

### Step 2: Analyze Testability

**Primitive:** investigate

**Purpose:** Identify what needs to be tested at each level

**Inputs:**
- Code/feature from Step 1
- Test conventions from Step 1

**Outputs:**
- **Units:** Individual functions/methods to test in isolation
- **Integrations:** Component interactions to test
- **Boundaries:** External dependencies (APIs, databases, files)
- **Edge cases:** Boundary conditions, error states, empty inputs
- **Happy paths:** Primary success scenarios

**Testability analysis:**

| Aspect | Questions to Answer |
|--------|---------------------|
| Units | What functions have logic worth testing? What are their inputs/outputs? |
| Integrations | Which components interact? What contracts exist between them? |
| Boundaries | What external systems are called? How can they fail? |
| Edge cases | What happens at boundaries? Empty inputs? Max values? Concurrent access? |
| Happy paths | What's the primary success scenario? |

### Step 3: Design Test Strategy

**Primitive:** design

**Purpose:** Plan test distribution following test pyramid principles

**Inputs:**
- Testability analysis from Step 2
- Test conventions from Step 1

**Outputs:**
- Test pyramid distribution (how many tests at each level)
- Test cases per level (what to test)
- Mocking strategy (what to mock and how)
- Coverage targets (if applicable)

**Test pyramid:**

```
        /\
       /  \      E2E Tests (few)
      /----\     - Critical user journeys
     /      \    - Slow, expensive
    /--------\   Integration Tests (some)
   /          \  - Component interactions
  /------------\ - Medium speed
 /              \ Unit Tests (many)
/----------------\ - Individual functions
                   - Fast, isolated
```

**Design considerations:**

| Level | Quantity | Speed | What to Test |
|-------|----------|-------|--------------|
| Unit | Many | Fast | Individual functions, edge cases, error handling |
| Integration | Some | Medium | Component interactions, API contracts, database operations |
| E2E | Few | Slow | Critical user journeys, smoke tests |

### Step 4: Identify Test Cases

**Primitive:** brainstorm

**Purpose:** Enumerate specific test cases for each level

**Inputs:**
- Test strategy from Step 3
- Testability analysis from Step 2

**Outputs:**
- Test case list per level
- For each test case:
  - **What** to test (function, endpoint, flow)
  - **Input** variations (valid, invalid, edge)
  - **Expected** behavior (return value, side effect, exception)
  - **Dependencies** to mock or stub

**Test case patterns:**

**For functions:**
- Valid input → expected output
- Invalid input → appropriate error
- Edge cases → correct handling
- Null/empty input → graceful handling

**For integrations:**
- Component A calls B → correct behavior
- Component A sends bad data to B → error handling
- Timeout/failure in B → A handles gracefully

**For E2E:**
- Complete user flow → success
- User flow with error → appropriate feedback

### Step 5: Generate Output

**Primitive:** implement

**Purpose:** Produce test strategy document or test code structure

**Inputs:**
- Test cases from Step 4
- Test conventions from Step 1

**Outputs:** One of:
- **Strategy document** — For planning and review
- **Test code skeleton** — For implementation

---

## Output Formats

### Format A: Test Strategy Document

```markdown
## Test Strategy: [Feature Name]

### Overview
[Brief description of what's being tested]

### Test Pyramid

#### Unit Tests
| Component | Test Cases | Priority |
|-----------|------------|----------|
| `function_name()` | Valid input, invalid input, boundary values | High |

#### Integration Tests
| Integration | Test Cases | Priority |
|-------------|------------|----------|
| API → Database | CRUD operations, error handling | Medium |

#### E2E Tests
| User Journey | Test Cases | Priority |
|--------------|------------|----------|
| User registration | Complete flow, validation errors | High |

### Coverage Targets
- Unit: 80%+
- Integration: Critical paths
- E2E: Happy paths only

### Dependencies to Mock
| Dependency | Why Mock | Mock Strategy |
|------------|----------|---------------|
| External API | Unreliable, slow | HTTP mock library |

### Edge Cases
- Empty inputs
- Maximum length inputs
- Concurrent access
- Network failures
```

### Format B: Test Code Skeleton

```python
# tests/test_[feature].py
"""Tests for [Feature Name]"""

import pytest
from unittest.mock import Mock, patch

from src.feature import FeatureClass, helper_function


class TestHelperFunction:
    """Unit tests for helper_function()"""

    def test_valid_input_returns_expected(self):
        """Test happy path with valid input."""
        # Arrange
        input_data = "valid"

        # Act
        result = helper_function(input_data)

        # Assert
        assert result == "expected"

    def test_empty_input_raises_value_error(self):
        """Test error handling for empty input."""
        with pytest.raises(ValueError):
            helper_function("")

    @pytest.mark.parametrize("input,expected", [
        ("case1", "result1"),
        ("case2", "result2"),
    ])
    def test_multiple_valid_inputs(self, input, expected):
        """Test various valid inputs."""
        assert helper_function(input) == expected


class TestFeatureClass:
    """Unit tests for FeatureClass"""

    @pytest.fixture
    def feature_instance(self):
        """Create a FeatureClass instance for testing."""
        return FeatureClass(config={})

    def test_method_with_dependency_mocked(self, feature_instance):
        """Test method with external dependency mocked."""
        with patch.object(feature_instance, 'external_service') as mock:
            mock.call.return_value = "mocked"
            result = feature_instance.method_using_service()
            mock.call.assert_called_once()
```

---

## Preconditions

**Must be provided:**
- feature or code: what to design tests for

**Self-satisfiable:**
- test conventions: read from existing tests
- test framework: detect from project (pytest, jest, etc.)

---

## Postconditions

**Success:**
- Testability analyzed at all levels
- Test strategy follows pyramid principles
- Test cases identified for each level
- Output produced (strategy doc or code skeleton)

**Failure (blocked):**
- Feature/code doesn't exist or isn't specified
- Unable to determine test framework (ask user)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify which steps are complete
   - Resume from next step
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1-2 (Orient/Analyze):** Safe to re-run
- **Step 3-4 (Design/Cases):** If partial, read existing and continue
- **Step 5 (Generate):** Safe to re-run; regenerate from test cases

---

## On Validation Failure

This workflow produces a design artifact, so validation failures are minimal. However:

**If test strategy seems incomplete:**
1. Review testability analysis for gaps
2. Check if all pyramid levels are covered
3. Verify edge cases are identified
4. Add missing test cases

---

## Quality Gates

| Step | Gate | Purpose |
|------|------|---------|
| Step 2 | Testability complete | All aspects analyzed (units, integrations, boundaries, edges) |
| Step 4 | Test cases identified | Each level has concrete test cases |

**User choice gate:** After Step 4, ask user: "Strategy document or test code skeleton?"

---

## Customization Points

When adapting this workflow for your project:

### Repository Type

**Prototype:**
- Focus on happy path tests
- Minimal edge case coverage
- Skip integration tests
- Fast, light coverage

**Production:**
- Full test pyramid
- Comprehensive edge cases
- Integration and e2e tests
- Coverage targets enforced

**Library:**
- Extensive API testing
- Public interface coverage
- Property-based testing consideration
- Backward compatibility tests

### Test Framework Conventions

- Framework: pytest, jest, rspec, go test, etc.
- File naming: test_*.py, *.test.js, *_spec.rb
- Directory structure: tests/, __tests__/, spec/
- Fixtures and factories patterns
- Mock library conventions

### Output Preferences

- Strategy doc only (for planning)
- Code skeleton only (for implementation)
- Both (comprehensive)

---

## Example Usage

**User:** "Design tests for the new payment processing module"

**Workflow execution:**

1. **Orient** — Read payment module, identify:
   - Functions: process_payment(), validate_card(), calculate_fees()
   - Dependencies: Payment gateway API, database
   - Test framework: pytest with fixtures
2. **Analyze testability:**
   - Units: Each function's logic
   - Integrations: Payment gateway calls, database writes
   - Boundaries: Gateway API, card validation service
   - Edge cases: Invalid cards, insufficient funds, network timeouts
3. **Design strategy:**
   - Unit: 15 tests (validation, calculation, error handling)
   - Integration: 5 tests (gateway calls, database operations)
   - E2E: 2 tests (successful payment, failed payment)
4. **Identify cases:**
   - `test_process_payment_success`
   - `test_process_payment_invalid_card`
   - `test_process_payment_gateway_timeout`
   - ...etc
5. **Generate output:** Strategy doc or test code skeleton per user preference

**Outcome:** Comprehensive test plan ready for implementation.

---

## Best Practices

- **Follow the pyramid** — Many unit, some integration, few e2e
- **Test behavior, not implementation** — Focus on what, not how
- **Identify edges early** — Edge cases are where bugs hide
- **Mock at boundaries** — Isolate units from external dependencies
- **Name tests clearly** — Test name should describe what's being tested

---

## Common Anti-Patterns

**Anti-Pattern 1: Inverted pyramid**
- ❌ Many e2e tests, few unit tests
- ✅ Many unit tests, few e2e tests

**Anti-Pattern 2: Testing implementation**
- ❌ Testing that internal method was called
- ✅ Testing that correct output was produced

**Anti-Pattern 3: Missing edge cases**
- ❌ Only testing happy path
- ✅ Testing errors, boundaries, empty inputs

**Anti-Pattern 4: No mocking strategy**
- ❌ Tests call real external services
- ✅ Mock external dependencies for isolation

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [project_quality.md](../protocols/project_quality.md) — Quality assessment

---

## Related Primitives

This workflow composes these primitives:

1. [orient](../primitives/orient.md) — Understand code and test conventions
2. [investigate](../primitives/investigate.md) — Analyze testability
3. [design](../primitives/design.md) — Plan test strategy
4. [brainstorm](../primitives/brainstorm.md) — Enumerate test cases
5. [implement](../primitives/implement.md) — Generate output

---

## Related Workflows

- [feature_workflow](feature_workflow.md) — May invoke test strategy during design phase
- [bugfix_workflow](bugfix_workflow.md) — Creates regression tests (simpler than full strategy)
