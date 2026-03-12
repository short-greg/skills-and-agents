---
name: designing
description: >
  Detailing how a solution will work and be used. Produces technical approach for how to build something before implementation.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "design this", "how should we build this", "what's the architecture",
  "what are the tradeoffs", "design the API", "design the data model".
  keywords: architecting, structuring, modeling, interfacing, planning
---

# Designing

**Goal:** Produce concrete technical approach that is specific enough to implement from.

**Intent:** Prevent implementation failures by surfacing complexity, unknowns, and risks before they become expensive to fix in code.

**Scope:** Figuring out how to build something. Answers "how will we build this?" not "what steps will we take?"

---

## Key Results - KR

1. Approach is concrete enough to implement — structure, interfaces, behavior defined, risks mitigated, tradeoffs evaluated
2. User has confirmed design is acceptable

---

## Terms

**Design:** The technical approach specifying structure, behavior, interfaces, and decisions needed to implement a solution.

**Tradeoff:** A choice between competing concerns where improving one aspect may worsen another.

---

## Preconditions

**Required:** Problem or component to design

**Optional:**
- Project conventions (patterns, architectural decisions)
- Existing codebase (structure, dependencies, constraints)
- Requirements or spec if available

## Postconditions

**Success:** Concrete approach established, specific enough to implement from, risks mitigated, fits conventions

**Failure:** Too many unknowns, requirements unclear, constraints conflict. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Gather Context | KR1 | To discover constraints and conventions needed for design | Use when unknowns discovered during design or conventions/constraints unclear | Read `protocols/elicitation.md` and apply appropriate techniques to gather missing context from artifacts or user | In: Current unknowns (required), Available artifacts (optional) | Out: Context summary with constraints and conventions |
| Propose Alternatives | KR1 | To generate design options for evaluation | Use when no design options exist or more alternatives needed | Read `protocols/thinking.md` and apply appropriate techniques to generate multiple design approaches | In: Problem to design (required), Known constraints (optional) | Out: Set of design alternatives with key characteristics |
| Evaluate Alternatives and Tradeoffs | KR1 | To compare options and make design decisions with rationale | Use when multiple alternatives exist and decision needed | Read `protocols/thinking.md` and apply appropriate techniques to evaluate tradeoffs and document decision rationale | In: Design alternatives (required), Evaluation criteria (optional) | Out: Decision records with rationale |
| Design Structure | KR1 | To define components, interfaces, and data | Use when defining what the system is made of | Read `protocols/system_modularity.md` and `protocols/thinking.md` and apply appropriate techniques to specify components, interfaces, and data structures | In: Design approach (required), Quality requirements (optional) | Out: Component diagram with interface specifications |
| Design Process | KR1 | To specify behavior, flows, and state transitions | Use when defining how the system behaves | Read `protocols/thinking.md` and apply appropriate techniques to map data flows, states, and error handling | In: Structure design (required), Quality requirements (optional) | Out: Process specification with flows and state transitions |
| Identify and Mitigate Risks | KR1 | To surface risks, uncertainty, and complexity and determine mitigations | Use when design drafted and risks not yet assessed | Read `protocols/risk_management.md` and `protocols/thinking.md` and apply appropriate techniques to identify risks and determine mitigations | In: Design (required), Known constraints (optional) | Out: Risk register with mitigations |
| Output and Confirm | KR2 | To present design and get user sign-off | Use when design is ready for review | Read `protocols/pragmatics.md` and apply appropriate techniques to format design and request confirmation | In: Complete design (required) | Out: Formatted design with confirmation request |

---

## References

- [Parnas - Information Hiding (1972)](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)
- [UWaterloo - Modularity: Coupling, Cohesion, Information Hiding](https://student.cs.uwaterloo.ca/~cs446/F2003/Notes/Modularity.pdf)
- [Software Engineering: Design Principles](https://softengbook.org/chapter5)
- [Claude Architecture Principles](https://claude.ai/public/artifacts/77364999-2624-44a6-90bf-7513d8eeb675)
