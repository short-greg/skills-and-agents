---
name: designing
description: >
  Detailing how a solution will work and be used. Produces technical approach for how to build something before implementation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "design this", "how should we build this", "what's the architecture",
  "what are the tradeoffs", "design the API", "design the data model".
  keywords: architecting, structuring, modeling, interfacing, planning
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

- **thinking.md** — Must use for Write Use Cases First, Design Structure, Design Process, Evaluate Tradeoffs
- **system_modularity.md** — Must use for Design Structure
- **discipline.md** — Must use for Write Use Cases First, Design Structure, Design Process, Identify and Mitigate Risks, Evaluate Tradeoffs
- **criteria_setting.md** — Must use for Design Structure, Design Process (testable interface specs, observable behaviors)
- **software_quality.md** — Must use for Design Structure, Design Process (quality attributes affecting design)
- **instruction_giving.md** — Must use for Design Structure, Design Process (clear, implementable specs)
- **transparency.md** — Must use for Design Structure, Design Process, Evaluate Tradeoffs (document design artifact and decisions)
- **risk_management.md** — Must use for Identify and Mitigate Risks
- **interviewing.md** — Use for Elicit Context (when info incomplete)
- **pragmatics.md** — Use for Elicit Context, Confirm with User
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

MUST read and follow steps in `base.md`

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

### Elicit Context (→ KR1)

Execute context elicitation using `interviewing.md` (Open Questions, Constraint Discovery) and `pragmatics.md` (Directness Calibration) when project conventions, codebase context, or requirements not provided. Gather what's needed to design accurately before proceeding.

### Write Use Cases First (→ KR1)

Execute use case creation using `thinking.md` (Analytical) and `discipline.md` (MECE Enumeration) when starting design. Write 3-5 primary use cases as ideal API calls or interactions, enumerate systematically, identify natural boundaries from usage patterns.

### Design Structure (→ KR1)

Execute structure design using `system_modularity.md` (Cohesion, Coupling, Information Hiding), `thinking.md` (Systems), `discipline.md` (Coverage Tracking), `criteria_setting.md` (Observable Behavior), `software_quality.md` (Quality Attributes), `instruction_giving.md` (Explicit Language), and `transparency.md` (Documentation) when defining components. Enumerate ALL modules, interfaces, and data structures. Specify testable interface contracts. Consider quality attributes. Document design clearly for implementation.

### Design Process (→ KR1)

Execute process design using `thinking.md` (Systems), `discipline.md` (Coverage Tracking), `criteria_setting.md` (Observable Behavior, Threshold Setting), `software_quality.md` (Quality Requirements), `instruction_giving.md` (Explicit Language), and `transparency.md` (Documentation) when specifying behavior. Map ALL data flows with observable criteria, enumerate ALL valid states and transitions, specify ALL error handling. Document quality requirements (performance, reliability).

### Identify and Mitigate Risks (→ KR1)

Execute risk identification using `risk_management.md` (Uncertainty Indicators, Complexity Indicators) and `discipline.md` (Coverage Tracking) when surfacing design risks. Check ALL components for complexity, unknowns, fragile assumptions, edge cases, security concerns. For each risk, determine concrete mitigation.

### Evaluate Tradeoffs (→ KR1)

Execute tradeoff evaluation using `thinking.md` (Counterfactual), `transparency.md` (Decision Recording), and `discipline.md` (Coverage Tracking) when making design decisions. Enumerate ALL key decisions, state alternatives considered for each, evaluate tradeoffs, document rationale for choices.

### Confirm with User (→ KR2)

Execute design confirmation using `pragmatics.md` (Recommended Option, Option Presentation) when presenting design for review. Frame as recommended approach with reasoning, present with enough precision to implement from, verify acceptability before implementation begins.

---

## Additional Notes and Terms

None

---

## References

- [Parnas - Information Hiding (1972)](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf)
- [UWaterloo - Modularity: Coupling, Cohesion, Information Hiding](https://student.cs.uwaterloo.ca/~cs446/F2003/Notes/Modularity.pdf)
- [Software Engineering: Design Principles](https://softengbook.org/chapter5)
- [Claude Architecture Principles](https://claude.ai/public/artifacts/77364999-2624-44a6-90bf-7513d8eeb675)
