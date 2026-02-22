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
  - reasoning_patterns
---

# Test Strategy Workflow

**Goal:** Design comprehensive test strategies applying test pyramid principles, identifying test cases, and producing strategy documents or test code structures.

**Intent:** Ensure features have thoughtful test coverage before implementation by systematically analyzing testability and applying test pyramid principles. Prevent ad-hoc testing that misses edge cases or has poor coverage distribution.

**Scope:** Test design and planning: understand code to test, analyze testability, design strategy following test pyramid, identify test cases, produce output.

---

## Workflow Type

**Type:** Deterministic

**Style:** Hybrid (declarative goals with imperative pyramid analysis)

---

## Key Results

1. Testability analyzed at all levels (unit, integration, e2e)
2. Test strategy follows pyramid principles (many unit, some integration, few e2e)
3. Test cases identified for each level
4. Edge cases and error scenarios covered
5. Output produced (strategy doc or code skeleton)

---

## Available Primitives

`orient`, `investigate`, `design`, `brainstorm`, `implement`

---

## Constraints

- Follow test pyramid (many unit, some integration, few e2e)
- Identify edge cases and error scenarios, not just happy path
- Test behavior, not implementation details
- Mock at boundaries (external dependencies)
- Output must be actionable (specific test cases, not vague descriptions)

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Test scope** — What needs testing? Full feature or specific component?
2. **Pyramid distribution** — Heavy unit testing? More integration needed?
3. **Mocking strategy** — What external dependencies need isolation?
4. **Output format** — Strategy doc for planning or code skeleton for implementation?

Output reasoning before proceeding.

---

## Execution

Execute by selecting and sequencing primitives to achieve key results.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand code/feature to test, project test conventions, existing patterns.

### Phase 2: Analyze Testability

**Primitive:** `investigate` — See `primitives/investigate.md`

Identify what needs testing at each level:
- **Units:** Functions/methods to test in isolation
- **Integrations:** Component interactions
- **Boundaries:** External dependencies
- **Edge cases:** Boundary conditions, error states, empty inputs
- **Happy paths:** Primary success scenarios

### Phase 3: Design Strategy

**Primitive:** `design` — See `primitives/design.md`

Plan test distribution following pyramid:
- Unit tests (many): Individual functions, edge cases, error handling
- Integration tests (some): Component interactions, API contracts
- E2E tests (few): Critical user journeys

### Phase 4: Identify Test Cases

**Primitive:** `brainstorm` — See `primitives/brainstorm.md`

Enumerate specific test cases for each level:
- What to test
- Input variations (valid, invalid, edge)
- Expected behavior
- Dependencies to mock

### Phase 5: Generate Output

**Primitive:** `implement` — See `primitives/implement.md`

Produce one of:
- **Strategy document** — For planning and review
- **Test code skeleton** — For implementation

**Gate:** Ask user preference before generating.

---

## Preconditions

**Must be provided:** Feature or code to design tests for

**Self-satisfiable:** Test conventions, test framework (read from existing tests)

---

## Postconditions

**Success:** Testability analyzed, strategy follows pyramid, test cases identified, output produced.

**Failure:** Feature/code doesn't exist, unable to determine test framework (ask user).

---

## Recovery

Per `recovery` protocol — check for existing trace on startup, resume from last completed step.

**Step-specific notes:**
- Analysis steps safe to re-run
- If partial test cases exist, read and continue
- Output generation is idempotent

---

## Test Pyramid

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

---

## Customization Points

**Prototype:** Focus on happy path, minimal edge coverage, skip integration tests.

**Production:** Full pyramid, comprehensive edge cases, coverage targets enforced.

**Library:** Extensive API testing, property-based testing consideration, backward compatibility tests.
