**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: test-strategy-workflow
description: >
  Use when designing test strategies for features or code changes.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Approach is reasoned about before starting analysis
2. Testability is understood — units, integrations, boundaries, edge cases identified
3. Strategy follows pyramid — distribution favors unit tests, with fewer integration and e2e tests
4. Test cases are specific — each case has what to test, inputs, expected behavior
5. Edge cases are covered — boundary conditions, error scenarios, invalid inputs identified
6. Output is actionable — strategy doc or code skeleton produced based on user preference

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work, track which analysis phases complete vs remain
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task, analysis tasks safe to re-run, if partial test cases exist read and continue
3. Follow test pyramid per `quality.md` — many unit, some integration, few e2e tests
4. Identify edge cases and error scenarios, not just happy path
5. Test behavior, not implementation details
6. Mock at boundaries (external dependencies)
7. Output must be actionable — specific test cases with inputs and expected behavior, not vague descriptions
8. Per `goals_and_objectives.md` — ensure test strategy achieves coverage goals
9. Iterate up to 2 times if strategy needs revision

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Feature or code to design tests for

**Elicit if not provided:**
- Test conventions (read from existing tests)
- Test framework (read from project dependencies)
- Output format preference (strategy doc or code skeleton)

**Optional:** Coverage targets, specific test levels to focus on

## Postconditions

The resulting state after the skill is finished.

**Success:** Testability analyzed, strategy follows pyramid, test cases identified, output produced.

**Failure:** Feature/code doesn't exist, unable to determine test framework, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason About Strategy
2. Understand Context
3. Analyze Testability
4. Design Strategy
5. Identify Test Cases
6. Generate Output

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Strategy (→ KR1)

Per `reasoning.md` — before beginning, reason about:

1. **Test scope** — What needs testing? Full feature or specific component?
2. **Pyramid distribution** — Heavy unit testing? More integration needed?
3. **Mocking strategy** — What external dependencies need isolation?
4. **Output format** — Strategy doc for planning or code skeleton for implementation?

Output your reasoning.

### Understand Context (→ KR1, KR2)

Use `orient` primitive. Understand code/feature to test, project test conventions, existing patterns.

### Analyze Testability (→ KR2, KR5)

Use `investigate` primitive. Identify what needs testing at each level:

- **Units:** Functions/methods to test in isolation
- **Integrations:** Component interactions
- **Boundaries:** External dependencies
- **Edge cases:** Boundary conditions, error states, empty inputs
- **Happy paths:** Primary success scenarios

### Design Strategy (→ KR3)

Use `design` primitive. Plan test distribution following pyramid:

- Unit tests (many): Individual functions, edge cases, error handling
- Integration tests (some): Component interactions, API contracts
- E2E tests (few): Critical user journeys

### Identify Test Cases (→ KR4, KR5)

Use `brainstorm` primitive. Enumerate specific test cases for each level:

- What to test
- Input variations (valid, invalid, edge)
- Expected behavior
- Dependencies to mock

### Generate Output (→ KR6)

Use `implement` primitive. Produce one of:

- **Strategy document** — For planning and review
- **Test code skeleton** — For implementation

**Gate:** Ask user preference before generating.

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand code/feature to test, project test conventions, existing patterns
- `investigate` — Analyze testability: units, integrations, boundaries, edge cases
- `design` — Plan test distribution following pyramid principles
- `brainstorm` — Enumerate specific test cases for each level
- `implement` — Produce strategy document or test code skeleton

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

**Test Pyramid:**

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

**Customization Points:**

- **Prototype:** Focus on happy path, minimal edge coverage, skip integration tests
- **Production:** Full pyramid, comprehensive edge cases, coverage targets enforced
- **Library:** Extensive API testing, property-based testing consideration, backward compatibility tests
