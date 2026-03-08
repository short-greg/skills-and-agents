---
name: designing
description: >
  Detailing how a solution will work and be used. Produces technical approach for how to build something before implementation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "design this", "how should we build this", "what's the architecture",
  "what are the tradeoffs", "design the API", "design the data model".
  keywords: architecting, structuring, modeling, interfacing, planning
---

# Designing

**Goal:** Produce concrete technical approach that is specific enough to implement from.

**Intent:** Prevent implementation failures by surfacing complexity, unknowns, and risks before they become expensive to fix in code.

**Scope:** Figuring out how to build something. Answers "how will we build this?" not "what steps will we take?"

---

## Table of Contents

- [Key Results](#key-results---kr) — Success criteria for this mode
- [Requirements](#requirements-and-constraints---req) — Rules and constraints to follow
- [Steps](#steps) — Reference to base mode execution
- [Terms](#terms) — Key vocabulary and definitions
- [Preconditions](#preconditions) — What's needed before starting
- [Postconditions](#postconditions) — What's delivered upon completion
- [Actions](#possible-actions) — Concrete steps to achieve results
- [Notes](#additional-notes-and-terms) — Additional context and details
- [References](#references) — External documentation and resources

## Key Results - KR

1. Approach is concrete enough to implement — structure, interfaces, risks mitigated, tradeoffs evaluated
2. User has confirmed design is acceptable

## Requirements and Constraints - REQ

1. Start with use cases — design from how the system will be used
2. Apply information hiding — hide design decisions likely to change
3. Identify and mitigate risks before implementation
4. Document design decisions and rationale

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** Problem or component to design

**Optional:**
- Project conventions (patterns, architectural decisions)
- Existing codebase (structure, dependencies, constraints)
- Requirements or spec if available
- Prior designs, performance requirements, UI designs

## Postconditions

**Success:** Concrete approach established, specific enough to implement from, risks mitigated, fits conventions

**Failure:** Too many unknowns, requirements unclear, constraints conflict. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Elicit Context (→ KR1)

**Goal:** Gather information needed for accurate design

**When:** Project conventions, codebase context, or requirements not provided

**Protocols:** `protocols/elicitation.md`, `protocols/pragmatics.md`

**Instructions:** Use interviewing techniques to discover constraints and context. Apply pragmatic communication patterns to gather what's needed to design accurately before proceeding.

**Inputs:**
- Problem or component to design (required)
- Known context (optional)

**Default Output:** Context summary with conventions, constraints, and requirements

---

### Write Use Cases First (→ KR1)

**Goal:** Define how the system will be used before designing internals

**When:** Starting design

**Protocols:** `protocols/thinking.md`, `protocols/discipline.md`

**Instructions:** Write 3-5 primary use cases as ideal API calls or interactions, enumerate systematically, identify natural boundaries from usage patterns. Use analytical thinking and MECE enumeration.

**Inputs:**
- Requirements or problem statement (required)
- User context (optional)

**Default Output:** Use cases as API calls or interactions

---

### Design Structure (→ KR1)

**Goal:** Define components, interfaces, and data structures

**When:** Defining components

**Protocols:** `protocols/system_modularity.md`, `protocols/thinking.md`, `protocols/discipline.md`, `protocols/criteria_setting.md`, `protocols/software_quality.md`, `protocols/transparency.md`

**Instructions:** Enumerate ALL modules, interfaces, and data structures. Apply cohesion, coupling, and information hiding principles. Specify testable interface contracts. Consider quality attributes. Document design clearly for implementation.

**Inputs:**
- Use cases (required)
- Project conventions (optional)
- Quality requirements (optional)

**Default Output:** Component diagram with interface specifications

---

### Design Process (→ KR1)

**Goal:** Specify behavior, data flows, and state transitions

**When:** Specifying behavior

**Protocols:** `protocols/thinking.md`, `protocols/discipline.md`, `protocols/criteria_setting.md`, `protocols/software_quality.md`, `protocols/transparency.md`

**Instructions:** Map ALL data flows with observable criteria, enumerate ALL valid states and transitions, specify ALL error handling. Document quality requirements (performance, reliability). Use systems thinking and coverage tracking.

**Inputs:**
- Structure design (required)
- Quality requirements (optional)

**Default Output:** Process specification with data flows and state transitions

---

### Identify and Mitigate Risks (→ KR1)

**Goal:** Surface design risks and determine mitigations

**When:** Surfacing design risks

**Protocols:** `protocols/risk_management.md`, `protocols/discipline.md`

**Instructions:** Check ALL components for complexity, unknowns, fragile assumptions, edge cases, security concerns. For each risk, determine concrete mitigation. Use risk assessment techniques and coverage tracking.

**Inputs:**
- Design (required)
- Known constraints (optional)

**Default Output:** Risk register with mitigations

---

### Evaluate Tradeoffs (→ KR1)

**Goal:** Make and document design decisions

**When:** Making design decisions

**Protocols:** `protocols/thinking.md`, `protocols/transparency.md`, `protocols/discipline.md`

**Instructions:** Enumerate ALL key decisions, state alternatives considered for each, evaluate tradeoffs, document rationale for choices. Use counterfactual thinking and decision recording.

**Inputs:**
- Design options (required)
- Constraints (optional)

**Default Output:** Decision records with rationale

---

### Confirm with User (→ KR2)

**Goal:** Verify design acceptability before implementation

**When:** Presenting design for review

**Protocol:** `protocols/pragmatics.md`

**Instructions:** Frame as recommended approach with reasoning, present with enough precision to implement from, verify acceptability before implementation begins. Use option presentation techniques.

**Inputs:**
- Complete design (required)

**Default Output:** Confirmation request with design summary

---

## Additional Notes and Terms

None

---

## References

- [Parnas - Information Hiding (1972)](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)
- [UWaterloo - Modularity: Coupling, Cohesion, Information Hiding](https://student.cs.uwaterloo.ca/~cs446/F2003/Notes/Modularity.pdf)
- [Software Engineering: Design Principles](https://softengbook.org/chapter5)
- [Claude Architecture Principles](https://claude.ai/public/artifacts/77364999-2624-44a6-90bf-7513d8eeb675)
