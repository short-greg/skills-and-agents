---
name: designing
description: >
  Produces technical approach for how to build something before implementation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "design this", "how should we build this", "what's the architecture",
  "what are the tradeoffs", "design the API", "design the data model".
argument-hint: "[component or problem to design]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Designing

**Goal:** Produce concrete technical approach that is specific enough to implement from.

**Intent:** Prevent implementation failures by surfacing complexity, unknowns, and risks before they become expensive to fix in code.

**Scope:** Figuring out how to build something. Answers "how will we build this?" not "what steps will we take?"

---

## Key Results - KR

1. Approach is concrete enough to implement — structure, interfaces, risks mitigated, tradeoffs evaluated
2. User has confirmed design is acceptable

## Requirements and Constraints - REQ

1. Start with use cases — design from how the system will be used
2. Apply information hiding — hide design decisions likely to change
3. Identify and mitigate risks before implementation
4. Document design decisions and rationale

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **modularity.md** - Must use for use-case driven design and information hiding
- **risk_management.md** - Must use for identifying and mitigating risks
- **documentation.md** - Must use for documenting design decisions
- **instructions.md** - Must use for writing clear instructions for implementers
- **checklists.md** - Must use to ensure completeness
- **quality.md** - Use when evaluating design quality
- **reasoning.md** - Use when evaluating tradeoffs
- **tracking.md** - Use when tracking design aspects complete vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Problem or component to design

**Elicit if not provided:**
- Project conventions (patterns, architectural decisions)
- Existing codebase (structure, dependencies, constraints)
- Requirements or spec if available

**Optional:** Prior designs, performance requirements, UI designs

## Postconditions

**Success:** Concrete approach established, specific enough to implement from, risks mitigated, fits conventions

**Failure:** Too many unknowns, requirements unclear, constraints conflict

---

## Actions

Select based on context. Each action shows which KR it serves.

### Write Use Cases First (→ KR1)
Usage must drive design. Write 3-5 primary use cases as ideal API calls or interactions before designing components. Identify natural boundaries from the flow.

### Design Structure (→ KR1)
Component boundaries must be explicit. Define:
- File/module hierarchy and dependency graph
- Data model (structures, relationships, ownership)
- Interfaces (function signatures, API shapes, contracts)

Apply `modularity.md`: high cohesion within, loose coupling between. Hide what changes frequently.

### Design Process (→ KR1)
Behavior must be specified. Define:
- Data flow (how data moves through system)
- State transitions (valid states and transitions)
- Error handling (how failures propagate and recover)

### Identify and Mitigate Risks (→ KR1)
Risks must be resolved before implementation. Identify complexity, unknowns, fragile assumptions, edge cases, security concerns. For each risk, determine concrete mitigation.

### Evaluate Tradeoffs (→ KR1)
Design decisions must be justified. For key decisions, state alternatives considered, tradeoffs evaluated, and rationale for choice.

### Confirm with User (→ KR2)
Design must be agreed. Present approach with enough precision to implement from. Verify acceptability before implementation begins.

---

## Additional Notes and Terms

None

---

## References

- [Parnas - Information Hiding (1972)](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)
- [UWaterloo - Modularity: Coupling, Cohesion, Information Hiding](https://student.cs.uwaterloo.ca/~cs446/F2003/Notes/Modularity.pdf)
- [Software Engineering: Design Principles](https://softengbook.org/chapter5)
- [Claude Architecture Principles](https://claude.ai/public/artifacts/77364999-2624-44a6-90bf-7513d8eeb675)
