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
---

# Design

**Goal:** Produce a concrete technical approach that mitigates known risks and is specific enough to implement from.

**Intent:** Prevent implementation failures caused by insufficient upfront thinking. Design surfaces complexity, unknowns, and risks — then resolves them — before they become expensive to fix in code.

**Scope:** Covers how to build — structure, interfaces, data models, process flows, UI, dependencies, conventions, tradeoffs, risks, and mitigations. Does not cover what to build (that is `define`), the order of actions (that is `plan`), or the actual implementation (that is `implement`).

---

## Preconditions

**Must be provided:**
- problem or component to design: what needs to be designed — ask if not clear from context

**Self-satisfiable:**
- project conventions: read project docs, style guides, architectural patterns in use — critical for producing designs that fit the system
- existing codebase: read relevant files to understand current structure, dependencies, and constraints
- existing definition: read requirements or spec if available

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
- Too many unknowns remain — `investigate` is needed before design can proceed
- Requirements are unclear — `define` is needed first
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
7. The design fits existing conventions and introduces no avoidable coupling or cycles
8. User has confirmed the design is acceptable

---

## Required Actions

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in this domain would approach designing
this component or system. Cover: their strategy, what they would do first and why,
alternatives they would consider, fallbacks if the primary approach fails, common mistakes
to avoid, and what a high-quality design looks like for this type of problem.
This is not a plan — it is meta-reasoning about approach.

Output this reasoning before proceeding.

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
- **identify unknowns**: surface things that need `investigate` before design can be finalized — blocks the design until resolved

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
