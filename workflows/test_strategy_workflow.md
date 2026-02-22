---
name: test-strategy-workflow
description: >
  Use when designing test strategies for features or code changes.
  Triggers on: "design tests for", "test strategy", "what tests do I need".
argument-hint: "[feature or code to design tests for]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, TodoWrite
---

# Test Strategy Workflow

**Goal:** Design comprehensive test strategies applying test pyramid principles, identifying test cases, and producing strategy documents or test code structures.

**Intent:** Ensure features have thoughtful test coverage before implementation by systematically analyzing testability and applying test pyramid principles. Prevent ad-hoc testing that misses edge cases or has poor coverage distribution.

**Scope:** Test design and planning: understand code to test, analyze testability, design strategy following test pyramid, identify test cases, produce output.

---

## Key Results

1. Approach is reasoned about before starting analysis
2. Testability is understood — units, integrations, boundaries, edge cases identified
3. Strategy follows pyramid — distribution favors unit tests, with fewer integration and e2e tests
4. Test cases are specific — each case has what to test, inputs, expected behavior
5. Edge cases are covered — boundary conditions, error scenarios, invalid inputs identified
6. Output is actionable — strategy doc or code skeleton produced based on user preference

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which analysis phases are complete vs remain
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist after reasoning about the test strategy. Update it dynamically.
- `reasoning.md` — Reason about approach before starting. Verify thoroughness after.
- `goals_and_objectives.md` — Ensure test strategy achieves coverage goals.
- `quality.md` — Consider quality dimensions: correctness, reliability, completeness.

---

## Available Primitives

Primitives are atomic cognitive actions in `skills/`. Use these to design tests. If you do not understand a primitive, read it before using it.

- `orient` — Understand code/feature to test, project test conventions, existing patterns.
- `investigate` — Analyze testability: units, integrations, boundaries, edge cases.
- `design` — Plan test distribution following pyramid principles.
- `brainstorm` — Enumerate specific test cases for each level.
- `implement` — Produce strategy document or test code skeleton.

---

## Constraints

- Follow test pyramid (many unit, some integration, few e2e)
- Identify edge cases and error scenarios, not just happy path
- Test behavior, not implementation details
- Mock at boundaries (external dependencies)
- Output must be actionable (specific test cases, not vague descriptions)

---

## Tasks

Execute these tasks to achieve the key results. Select and sequence based on your reasoning.

### Reason About Strategy

Per `reasoning.md` — before beginning, reason about:

1. **Test scope** — What needs testing? Full feature or specific component?
2. **Pyramid distribution** — Heavy unit testing? More integration needed?
3. **Mocking strategy** — What external dependencies need isolation?
4. **Output format** — Strategy doc for planning or code skeleton for implementation?

Output your reasoning.

### Understand Context

Use `orient` primitive. Understand code/feature to test, project test conventions, existing patterns.

### Analyze Testability

Use `investigate` primitive. Identify what needs testing at each level:

- **Units:** Functions/methods to test in isolation
- **Integrations:** Component interactions
- **Boundaries:** External dependencies
- **Edge cases:** Boundary conditions, error states, empty inputs
- **Happy paths:** Primary success scenarios

### Design Strategy

Use `design` primitive. Plan test distribution following pyramid:

- Unit tests (many): Individual functions, edge cases, error handling
- Integration tests (some): Component interactions, API contracts
- E2E tests (few): Critical user journeys

### Identify Test Cases

Use `brainstorm` primitive. Enumerate specific test cases for each level:

- What to test
- Input variations (valid, invalid, edge)
- Expected behavior
- Dependencies to mock

### Generate Output

Use `implement` primitive. Produce one of:

- **Strategy document** — For planning and review
- **Test code skeleton** — For implementation

**Gate:** Ask user preference before generating.

---

## Progress Tracking

Per `checklists.md` — create and maintain a checklist throughout execution.

**Rules:**
- Create checklist after reasoning about the strategy, based on what you learned
- Mark items complete immediately after finishing each task
- Report progress after each completed item

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

Per `recovery.md` — check for existing trace on startup, resume from last completed task.

**Task-specific notes:**
- Analysis tasks safe to re-run
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
