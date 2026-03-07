---
name: defining
description: >
  Specifying the problem, goal, and constraints of a project or task. Establishes requirements, success criteria, and definition of done.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "define this", "what are the requirements", "scope this out",
  "clarify requirements", "what should we create", "create a task description", "create a PRD".
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
2. Requirements are accurate, precise, and business oriented — in line with user requests
3. User has confirmed the definition
4. Uncertainty in requirements is minimized
5. Requirements are output in the required format

## Requirements and Constraints - REQ

1. Acceptance criteria must be testable
2. Interview user to clarify intent if ambiguous
3. Define key terms precisely enough to use as evaluation gates

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** Task or feature to define (can be vague initially)

**Optional:**
- User intent, goal, and requirements fully specified
- Required output format/style
- Existing context (codebase, docs, constraints)
- Prior definitions, technical constraints

## Postconditions

**Success:** Requirements documented with testable acceptance criteria, confirmed by user

**Failure:** Requirements could not be determined or unresolvable conflicts exist. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Output Intention (→ All KRs)

**Reference**: See `base_mode`

**When**: Before executing any other action

[COMMENT: include in base.md]
**Goal:** Communicate what actions will be executed and why

**When:** Before executing other actions

**Protocol:** `protocols/transparency.md`, `protocols/tracking_and_recovery.md`

**Instructions:** Output which actions you plan to execute and the reasoning for selecting them based on the current context and available information. Consider other possible actions as well.

**Inputs:**
- Available context
- Identified gaps

**Default Output:** Inline text, lightweight checklist of key results with actions

---

### Elicit Requirements (→ KR1, KR2, KR4)

**Goal:** Gather missing requirements information from available sources or identify what's needed

**When:** User intent, goal, or requirements are incomplete or unclear

**Protocol:** `protocols/transparency.md`

[COMMENT: I reworded this. If it should interview, then it will fail. We'll create the interviewing mode later]
**Instructions:** Output a detailed questionnaire or description of what's needed using appropriate framing. If interviewing is preferred/possible, output the request and signal failure so outer skill can handle elicitation.

**Inputs:**
- Task description (required)
- Known constraints (optional)
- Target audience (optional)

**Default Output:** Codeblock with structured questions

---

### Analyze Existing Context (→ KR1, KR2, KR4)

**Goal:** Understand what already exists to inform requirements

**When:** Information may be discoverable from existing artifacts without asking user

**Protocol:** `protocols/thinking.md`

**Instructions:** Review existing code, docs, and specs to understand what exists and what constraints apply. Choose appropriate analytical techniques from the protocol.

**Inputs:**
- Codebase paths (optional)
- Documentation locations (optional)

**Default Output:** Summary of findings

---

### Write User Stories (→ KR1, KR2)

**Goal:** Capture requirements from user perspective

**When:** Capturing user experience and outcomes

**Protocol:** `protocols/goal_setting.md`

**Instructions:** Read the protocol and choose appropriate goal-setting techniques to express requirements as user outcomes.

**Inputs:**
- User role/persona (optional)
- Context of use (optional)

**Default Output:** Formatted user stories

---

### Specify Acceptance Criteria (→ KR1, KR2)

**Goal:** Define testable pass/fail criteria for requirements

**When:** Establishing how completion will be verified

**Protocols:** `protocols/goal_setting.md`, `protocols/criteria_setting.md`

**Instructions:** Read the protocols and choose appropriate techniques for setting goals and establishing observable, testable criteria with explicit thresholds.

**Inputs:**
- Requirements/user stories (required)
- Quality attributes (optional)

**Default Output:** Formatted acceptance criteria

---

### Analyze Requirements (→ KR2, KR4)

**Goal:** Validate requirements for completeness, consistency, and feasibility

**When:** Requirements need validation before finalization

**Protocol:** `protocols/thinking.md`

**Instructions:** Read the protocol and choose appropriate thinking techniques to check for completeness, consistency, feasibility, and priority. Identify conflicts or gaps.

**Inputs:**
- Draft requirements (required)
- Constraints (optional)

**Default Output:** Analysis with identified issues

---

### State Out-of-Scope (→ KR1, KR4)

**Goal:** Define boundaries to prevent scope creep

**When:** Clarifying what is explicitly excluded

**Protocol:** `protocols/goal_setting.md`

**Instructions:** Read the protocol and use boundary-setting techniques to explicitly document what is not included.

**Inputs:**
- Scope boundaries (optional)
- Known exclusions (optional)

**Default Output:** List of out-of-scope items

---

### Output Definition (→ KR3, KR5)

**Goal:** Format and present the complete definition

**When:** Ready to output final requirements document

**Protocol:** `protocols/transparency.md`

**Instructions:** Read the protocol and choose appropriate techniques for the output style. Default is codeblock if style not specified.

**Inputs:**
- All requirement components (required)
- Output format preference (optional, default: codeblock)

**Default Output:** Codeblock or specified format

---

### Confirm with User (→ KR3)

**Goal:** Verify definition accuracy with user

**When:** Definition is ready for review

**Protocols:** `protocols/pragmatics.md`

**Instructions:** Read the protocol and choose appropriate techniques for presenting the definition and requesting confirmation. Paraphrase understanding, verify accuracy, resolve ambiguities.

**Inputs:**
- Complete definition (required)

**Default Output:** Confirmation request with definition

---

## Additional Notes

**Requirements Engineering phases:** Elicitation → Analysis → Documentation → Validation → Management (IEEE 29148)

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
- [Acceptance Criteria Best Practices](https://www.productplan.com/glossary/acceptance-criteria/)
- [User Stories - Mountain Goat Software](https://www.mountaingoatsoftware.com/articles/advantages-of-user-stories-for-requirements)
- [Anthropic - Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
