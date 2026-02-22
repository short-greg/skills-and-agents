---
name: design
description: >
  Use when figuring out how to build something before writing code. Triggers on:
  "design this", "how should we build this", "what's the architecture", "think through
  the approach", "what are the tradeoffs", "how do we handle X", "design the data model",
  "design the API", "design the schema", "design the UI", "what could go wrong",
  "mitigate the risks", "break this down technically".
argument-hint: "[component or problem to design]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
protocols:
  - tracking  # Track which design aspects are complete vs remain
  - recovery  # Resume design from where it was interrupted
  - reasoning_patterns  # Reason about design approach before starting, verify design completeness after
  - goals_and_objectives  # Ensure design achieves the defined success criteria
  - modularity  # Ensure designs have clear boundaries, minimal coupling, and single responsibilities
  - manage_complexity_uncertainty_risk  # Identify technical risks and unknowns, then resolve them before implementation
  - doc_maintenance  # Document design decisions and rationale for future reference
---

# Design

**Goal:** Produce a concrete technical approach that mitigates known risks and is specific enough to implement from.

**Intent:** Prevent implementation failures caused by insufficient upfront thinking. Design surfaces complexity, unknowns, and risks — then resolves them — before they become expensive to fix in code.

**Scope:** Figuring out how to build something before writing code. Includes: structural design (file/module hierarchy, dependency graphs, data models, interfaces), process design (data flow, state transitions, error handling, concurrency), test strategy (what to test, edge cases to cover, test boundaries), evaluating tradeoffs between alternatives, identifying technical risks and determining concrete mitigations, and ensuring the approach fits existing project conventions and architecture. Design is about technical approach — it answers "how will we build this?" not "what steps will we take?"

---

## Preconditions

**Must be provided:**
- problem or component to design: what needs to be designed — ask if not clear from context

**Self-satisfiable:**
- project conventions: read project docs, style guides, architectural patterns in use — critical for producing designs that fit the system
- existing codebase: read relevant files to understand current structure, dependencies, and constraints
- existing definition: read requirements or spec if available
- prior design: if a design already exists, read it as starting point — enables refinement when looping back after validation failure

**Non-essential:**
- performance requirements: if known, will constrain design choices
- external system constraints: APIs, schemas, protocols that must be conformed to
- UI designs: if not available, may need to be created by user or requested before structural design can be finalized

---

## Postconditions

**Success:**
- A concrete approach is established that is specific enough to implement from
- Structure and process are both addressed as needed
- Known risks are mitigated, not just listed
- Tradeoffs are resolved — a choice is made, not just alternatives enumerated
- The design fits project conventions and existing architecture

**Failure:**
- Too many unknowns remain — further investigation is needed before design can proceed
- Requirements are unclear — further definition is needed first
- Fundamental constraints conflict in a way that cannot be resolved without user input
- UI designs are required but not available and user must provide them

---

## Key Results

1. The approach is concrete enough to implement without further design decisions
2. Alternatives were considered and the chosen approach is justified
3. Known risks are mitigated — not just identified, but addressed
4. Structural design is complete: file/module hierarchy, dependency graph, key abstractions
5. Process design is complete where needed: data flow, state transitions, error handling, concurrency
6. Interfaces are precisely defined: function signatures, API shapes, data schemas, event contracts
7. Test strategy is defined: critical paths, edge cases, test types, test boundaries
8. The design fits existing conventions and introduces no avoidable coupling or cycles
9. User has confirmed the design is acceptable

---

## Required Actions

You must comply with these protocols. Review any you have not read:

- `protocols/tracking.md` — Track which design aspects are complete vs remain
- `protocols/recovery.md` — Resume design from where it was interrupted
- `protocols/reasoning_patterns.md` — Reason about design approach before starting, verify design completeness after
- `protocols/goals_and_objectives.md` — Ensure design achieves the defined success criteria
- `protocols/modularity.md` — Ensure designs have clear boundaries, minimal coupling, and single responsibilities
- `protocols/manage_complexity_uncertainty_risk.md` — Identify technical risks and unknowns, resolve them before implementation
- `protocols/doc_maintenance.md` — Document design decisions and rationale for future reference

**Confirm (REQUIRED LAST)**
Before declaring done, verify against each key result:
- Does the design satisfy each key result?
- If not, what is missing and what action addresses it?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Possible Actions

Select and sequence based on context and expert reasoning. Others may be used.

**Understanding**
- **read conventions**: read project docs, existing patterns, architectural decisions — prevents designs that violate established norms or introduce inconsistency
- **map dependencies**: understand what the component depends on and what depends on it — prevents tight coupling, cycles, and integration surprises
- **dissect the problem**: break the problem into sub-problems to understand its shape before designing — prevents over-engineering the wrong part or missing a key concern

**Structural Design** (shape of the system — prevents structural failure modes)
- **design file/module hierarchy**: define how code is organized across files and modules — prevents cyclical dependencies, unclear ownership, and navigation confusion
- **design dependency graph**: make inter-module and inter-service dependencies explicit — prevents cycles, hidden coupling, and brittle integration
- **design data model**: define data structures, relationships, ownership, and storage — prevents schema conflicts, redundancy, and data integrity issues
- **define interfaces**: specify public surfaces — function signatures, API shapes, event contracts, data schemas — prevents integration mismatches and leaky abstractions
- **request UI designs**: if UI is involved and no designs exist, surface this gap to the user — UI structure shapes data model and component hierarchy

**Process Design** (behavior of the system — prevents behavioral failure modes)
- **design data flow**: trace how data moves through the system — prevents lost updates, unintended mutations, and unclear ownership
- **design state transitions**: define valid states and transitions — prevents invalid states, race conditions, and unpredictable behavior
- **design error handling**: specify how failures propagate and are recovered — prevents silent failures, cascading errors, and poor user experience
- **design concurrency model**: identify parallel operations and synchronization points — prevents race conditions, deadlocks, and performance bottlenecks

**Risk Resolution**
- **identify risks**: surface technical risks — complexity, unknowns, fragile assumptions, performance cliffs, edge cases, security concerns
- **mitigate risks**: for each risk, determine a concrete mitigation — change the design, add a safeguard, or explicitly accept with rationale; the goal is resolution, not just awareness
- **identify unknowns**: surface things that need further investigation before design can be finalized — blocks the design until resolved

**Test Strategy** (what to test and why — guides implementation)
- **identify critical paths**: which flows must work correctly for the feature to be considered functional — these need tests
- **identify edge cases**: boundary conditions, empty inputs, invalid states, error scenarios — enumerate them so implementation covers them
- **determine test types**: what level of testing is appropriate — unit tests, integration tests, end-to-end tests, manual testing
- **identify test boundaries**: what is tested in isolation vs. what needs integration — clarifies mocking/stubbing strategy

**Output**
- **produce design document**: write the design with enough precision to implement from — include structural diagrams or pseudocode where ambiguity would otherwise exist; this is a primary deliverable, not a summary
- **review with user**: confirm the design is acceptable before implementation begins

---

## Use Cases

- Before implementing a non-trivial feature
- Designing a new API, data model, or system component
- Evaluating architectural options before committing
- Understanding and mitigating risks before writing code
- Ensuring a design fits project conventions and avoids coupling or cycles

---

## Tools
None

## Hooks
None
