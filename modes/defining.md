---
name: defining
description: >
  Specifying the problem, goal, and constraints of a project or task. Establishes requirements, success criteria, and definition of done.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "define this", "what are the requirements", "scope this out",
  "clarify requirements", "what should we create", "create a task description", "create a PRD".
  keywords: specifying, scoping, requirements, criteria, clarifying
---

# Defining

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring task is well-understood before solution is attempted. Bad definitions lead to building the wrong thing.

**Scope:** Establishing what to build and defining done. Answers "what are we building and how will we know it's done?" not "how will we build it?"

---

## Key Results - KR

1. Definition complete and accurate — problem stated, success defined, boundaries clear, uncertainty minimized
2. Definition confirmed and formatted — user has confirmed, output in required format

---

## Terms

**Definition:** The complete specification of what to build, including problem statement, success criteria, scope boundaries, and constraints.

**Success Criteria:** Observable conditions that indicate the work is complete and correct.

---

## Preconditions

**Required:** Task or feature to define (can be vague initially)

**Optional:**
- User intent, goal, and requirements
- Required output format/style
- Existing context (codebase, docs, constraints)

## Postconditions

**Success:** Definition documented with testable success criteria, confirmed by user

**Failure:** Definition could not be determined or unresolvable conflicts exist. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| State the Problem | KR1 | To articulate what we're solving and why | Use when problem is unclear or not yet articulated | Read and apply appropriate techniques from `protocols/goal_setting.md` to output a clear problem statement | In: Task description (required), User context (optional) | Out: Clear problem statement with context |
| Elicit Requirements | KR1 | To gather missing information from user or context | Use when requirements are incomplete or ambiguous | Read and apply appropriate techniques from `protocols/elicitation.md` to output questions or gather information from existing artifacts | In: Current definition state (required), Available artifacts (optional) | Out: Gathered requirements or structured questions |
| Define Success | KR1 | To specify how we know it's done | Use when success criteria not yet defined | Read and apply appropriate techniques from `protocols/criteria_setting.md` and `protocols/goal_setting.md` to output testable success criteria | In: Problem statement (required), Requirements (optional) | Out: Testable success criteria |
| Define Boundaries | KR1 | To specify scope and constraints | Use when scope is unclear or constraints not documented | Read and apply appropriate techniques from `protocols/goal_setting.md` to output what's in scope, out of scope, and constraints | In: Problem statement (required), Known constraints (optional) | Out: Scope boundaries and constraints |
| Validate Definition | KR1 | To check completeness, consistency, and feasibility | Use when definition drafted and needs validation | Read and apply appropriate techniques from `protocols/thinking.md` to check for gaps, conflicts, and feasibility issues | In: Draft definition (required) | Out: Validation results with identified issues |
| Output and Confirm | KR2 | To format, present, and get user sign-off | Use when definition is ready for review | Read and apply appropriate techniques from `protocols/pragmatics.md` to format definition and request confirmation | In: Complete definition (required), Output format preference (optional) | Out: Formatted definition with confirmation request |

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
- [Acceptance Criteria Best Practices](https://www.productplan.com/glossary/acceptance-criteria/)
- [User Stories - Mountain Goat Software](https://www.mountaingoatsoftware.com/articles/advantages-of-user-stories-for-requirements)
- [Anthropic - Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
