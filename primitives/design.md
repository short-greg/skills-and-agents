---
name: design
description: >
  Use when figuring out how to build something before writing code. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "design this", "how should we build this", "what's the architecture", "think through the approach", "what are the tradeoffs", "how do we handle X", "design the data model", "design the API", "design the schema", "design the UI", "what could go wrong", "mitigate the risks", "break this down technically".
argument-hint: "[component or problem to design]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Design

**Goal:** Produce concrete technical approach that mitigates known risks and is specific enough to implement from.

**Intent:** Prevent implementation failures caused by insufficient upfront thinking. Design surfaces complexity, unknowns, and risks — then resolves them — before they become expensive to fix in code.

**Scope:** Figuring out how to build something before writing code. Includes structural design (file/module hierarchy, dependency graphs, data models, interfaces), process design (data flow, state transitions, error handling, concurrency), test strategy (what to test, edge cases, test boundaries), evaluating tradeoffs between alternatives, identifying technical risks and determining concrete mitigations, and ensuring approach fits existing project conventions and architecture. Design answers "how will we build this?" not "what steps will we take?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Approach is concrete enough to implement (structure, process, interfaces defined, risks mitigated, alternatives evaluated, test strategy specified)
2. User has confirmed design is acceptable

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Track which design aspects complete vs remain per `tracking.md`
2. Ensure designs have clear boundaries, minimal coupling, single responsibilities per `modularity.md`
3. Identify technical risks and unknowns, resolve them before implementation per `risk_management.md`
4. Document design decisions and rationale per `documentation.md`
5. Design fits existing conventions and introduces no avoidable coupling or cycles

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Problem or component to design

**Elicit if not provided:**
- Project conventions (read project docs, style guides, architectural patterns)
- Existing codebase (read relevant files to understand current structure, dependencies, constraints)
- Existing definition (read requirements or spec if available)

**Optional:** Prior design (if exists, read as starting point), performance requirements, external system constraints, UI designs

## Postconditions

The resulting state after the primitive is finished.

**Success:** Concrete approach established that is specific enough to implement from. Structure and process addressed. Known risks mitigated. Tradeoffs resolved. Design fits project conventions and existing architecture.

**Failure:** Too many unknowns remain (further investigation needed), requirements unclear (further definition needed first), fundamental constraints conflict (cannot be resolved without user input), UI designs required but not available.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Design Structure and Process (→ KR1)

**Understanding:** Read conventions (project docs, existing patterns, architectural decisions). Map dependencies (understand what component depends on and what depends on it). Dissect problem (break into sub-problems).

**Structural Design:** Design file/module hierarchy, dependency graph, data model (structures, relationships, ownership, storage). Define interfaces (function signatures, API shapes, event contracts, data schemas). Request UI designs if needed and not available.

**Process Design:** Design data flow (how data moves through system), state transitions (valid states and transitions), error handling (how failures propagate and are recovered), concurrency model (parallel operations and synchronization points).

### Mitigate Risks and Plan Tests (→ KR1)

Identify risks (complexity, unknowns, fragile assumptions, performance cliffs, edge cases, security concerns). Mitigate risks (for each risk, determine concrete mitigation). Identify unknowns (surface things needing further investigation).

**Test Strategy:** Identify critical paths (flows that must work correctly), edge cases (boundary conditions, empty inputs, invalid states, error scenarios), determine test types (appropriate level of testing), identify test boundaries (what tested in isolation vs integration).

### Confirm with User (→ KR2)

Produce design document with enough precision to implement from. Review with user to confirm design is acceptable before implementation begins.

---

## Additional Notes and Terms

**Use cases:** Before implementing non-trivial feature, designing new API/data model/system component, evaluating architectural options, understanding and mitigating risks before writing code, ensuring design fits conventions and avoids coupling.
