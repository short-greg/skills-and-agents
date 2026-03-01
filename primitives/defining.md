---
name: defining
description: >
  Specifying the problem, goal, and constraints of a project or task. Establishes requirements, success criteria, and definition of done.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "define this", "what are the requirements", "scope this out",
  "clarify requirements", "what should we build", "create a PRD".
  keywords: specifying, scoping, requirements, criteria, clarifying
argument-hint: "[task or feature to define]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Defining

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring task is well-understood before solution is attempted. Bad definitions lead to building the wrong thing.

**Scope:** Establishing what to build and defining done. Answers "what are we building and how will we know it's done?" not "how will we build it?"

---

## Key Results - KR

1. Requirements complete — goal clear, acceptance criteria testable, out-of-scope stated
2. User has confirmed definition is accurate

## Requirements and Constraints - REQ

1. Acceptance criteria must be testable (pass/fail, no interpretation)
2. Interview user to clarify intent if ambiguous
3. Define key terms precisely enough to use as evaluation gates

---

## Protocols

- **goal_setting.md** — Must use for Write User Stories, Specify Acceptance Criteria, State Out-of-Scope
- **criteria_setting.md** — Must use for Specify Acceptance Criteria
- **interviewing.md** — Must use for Elicit Requirements, Analyze Existing Context, Confirm with User
- **pragmatics.md** — Use for Elicit Requirements, Confirm with User (directness calibration, presenting definition)
- **thinking.md** — Use for Analyze Requirements (consistency, feasibility reasoning)
- **tracking_and_recovery.md** — Must use for checklist and resuming after interruption

---

## Steps

Inherits from `base.md` — output lightweight checklist, resolve preconditions, plan actions, execute, report result.

---

## Preconditions

**Required:** Task or feature to define (can be vague initially)

**Elicit if not provided:**
- User intent (interview to understand what they actually want)
- Existing context (codebase, docs, constraints)

**Optional:** Prior definitions, technical constraints

## Postconditions

**Success:** Requirements documented with testable acceptance criteria, confirmed by user

**Failure:** Requirements could not be determined, unresolvable conflicts

---

## Actions

Select based on context. Each action shows which KR it serves.

### Elicit Requirements (→ KR1)

Execute requirements elicitation using `interviewing.md` (Open Questions, Success Definition, Constraint Discovery) and `pragmatics.md` (Directness Calibration) when user intent is unclear or incomplete. Ask targeted questions: What problem are you solving? Who is the user? What does success look like? What are the constraints?

### Analyze Existing Context (→ KR1)

Execute context analysis using `interviewing.md` (Inference First) when information may be discoverable without asking. Review existing code, docs, and specs to understand what exists and what constraints apply before asking user.

### Write User Stories (→ KR1)

Execute user story creation using `goal_setting.md` (Outcome Focus) when capturing user experience. Express as outcomes not activities. Use template: "As a [user] I want to [action], so that [benefit]." Include edge cases.

### Specify Acceptance Criteria (→ KR1)

Execute criteria specification using `goal_setting.md` (Outcome Focus) and `criteria_setting.md` (Observable Behavior, Threshold Setting) when defining done. For each requirement, write pass/fail criteria with observable behaviors and explicit thresholds. Focus on outcomes not implementation steps.

### Analyze Requirements (→ KR1)

Execute requirements analysis using `thinking.md` (Critical, Analytical) when validating before confirmation. Check for completeness (all necessary requirements captured?), consistency (no conflicts?), feasibility (achievable within constraints?), and priority (most important identified?).

### State Out-of-Scope (→ KR1)

Execute boundary definition using `goal_setting.md` (Boundary Setting) when clarifying scope. Define what is explicitly out of scope to prevent scope creep. Document exclusions clearly.

### Confirm with User (→ KR2)

Execute confirmation using `interviewing.md` (Understanding Validation) and `pragmatics.md` (Option Presentation) when definition is ready for review. Paraphrase understanding, present draft definition, verify accuracy, resolve ambiguities before finalizing.

---

## Additional Notes

**Requirements Engineering phases:** Elicitation → Analysis → Documentation → Validation → Management (IEEE 29148)

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
- [Acceptance Criteria Best Practices](https://www.productplan.com/glossary/acceptance-criteria/)
- [User Stories - Mountain Goat Software](https://www.mountaingoatsoftware.com/articles/advantages-of-user-stories-for-requirements)
- [Anthropic - Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
