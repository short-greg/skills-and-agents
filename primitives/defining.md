---
name: defining
description: >
  Specifying the problem, goal, and constraints of a project or task. Establishes requirements, success criteria, and definition of done.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "define this", "what are the requirements", "scope this out",
  "clarify requirements", "what should we create", "create a task description", "create a PRD".
  keywords: specifying, scoping, requirements, criteria, clarifying
argument-hint: "[task or feature to define]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

[COMMENT: I've added a variety of comments here. We need to refine this primitive and then update the 
guideline/template for primitives.
]

# Defining

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring task is well-understood before solution is attempted. Bad definitions lead to building the wrong thing.

**Scope:** Establishing what to build and defining done. Answers "what are we building and how will we know it's done?" not "how will we build it?"

---

## Key Results - KR

1. Requirements complete — goal clear, acceptance criteria testable, out-of-scope stated
2. Requirements are accurate and precise and business oriented - in line with user requests
3. User has confirmed the definition
4. Uncertainty in requirements is minimized
5. Requirements are output in the required format

## Requirements and Constraints - REQ

1. Acceptance criteria must be testable
2. Interview user to clarify intent if ambiguous
3. Define key terms precisely enough to use as evaluation gates

---

[COMMENT: I think this is not necessary, protocols are listed in the actions]
## Protocols

- **protocols/goal_setting.md** — Must use for Write User Stories, Specify Acceptance Criteria, State Out-of-Scope
- **protocols/criteria_setting.md** — Must use for Specify Acceptance Criteria
- **protocols/interviewing.md** — Must use for Elicit Requirements, Analyze Existing Context, Confirm with User
- **protocols/pragmatics.md** — Use for Elicit Requirements, Confirm with User (directness calibration, presenting definition)
- **protocols/thinking.md** — Use for Analyze Requirements (consistency, feasibility reasoning)
- **protocols/transparency.md** — Use for Elicit Requirements, Confirm with User (directness calibration, presenting definition)
- **protocols/tracking_and_recovery.md** — Must use for lightweight checklist and resuming after interruption

---

## Steps

MUST read and follow steps in `base.md`

---

## Preconditions

**Required:** Task or feature to define (can be vague initially)

**Optional:** A description of the required output.

[COMMENT:
Changed this to be optional. A request will be shared, but if the outer skill requires interviewing then the primitive will fail

Request must specify the goal and everythig that is needed to satisfy its requirements.

Let's remove the "Elicit" precondition and use optional

Rephrase like below.
]
**Optional:**
- User intent, goal, and document and requirements is fully provided to meet key results
- Existing context (codebase, docs, constraints)
- If not specified and wnat to do interview will fail, otherwise outputs a request

**Optional:** Prior definitions, technical constraints

## Postconditions

**Success:** Requirements documented with testable acceptance criteria, confirmed by user

[
  TODO: If it cannot succeed it outputs the information that it needs and its goal.

This is instead of it eliciting what it wants on its own.
]

**Failure:** Requirements could not be determined, unresolvable conflicts. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select or propose actions based on context. Each action shows which KR it serves.

[COMMENT: Use this description so it requires reading the protocol]
Each action specifies a protocol to use. When executing the action you MUST read that protocol if you haven't and MUST choose the appropriate techniques based on that protoocol to a achieve the key results of this skill.

[COMMENT: Let's simplify each action to be a bit more template oriented. I shared my ideas below in elicit requirements.
Also, before the actions were too prescriptive. We must be less prescriptive. 
Note: protocols must be referred to with protocols/<protocol name>.md
]

### Output Intention ()

[COMMENT: In general the intention, like actions to execute must be output.]

### Elicit Requirements (→ KR1/KR2)

Goal: Output requests for requirements 
When: <condition>
Protocol: `protocols/transparency.md`
Instructions: Output defailed questionairre/description of what's needed using recommendations where needed
Inputs: <input list with default values if not specified>
Default Output: Codeblock
Other: If preference for interviewing, then output this request and have the skill fail

### Analyze Existing Context (→ KR1)

Execute context analysis using `protocols/thinking.md` (Inference First) when information may be discoverable without asking. Review existing code, docs, and specs to understand what exists and what constraints apply before asking user.  

### Write User Stories (→ KR1)

Execute user story creation reading and using `protocols/goal_setting.md` by choosing the appropriate goal setting techniques when capturing user experience. 

### Specify Acceptance Criteria (→ KR1)

Execute criteria specification using `protocols/goal_setting.md` and `protocols/criteria_setting.md` by choosing the appropriate techniques setting goals and criteria.

### Analyze Requirements (→ KR1)

Execute requirements analysis using `protocols/thinking.md` by choosing techniques to   when validating before confirmation. 

### State Out-of-Scope (→ KR1)

Execute boundary definition using `protocols/goal_setting.md` (Boundary Setting) when clarifying scope. Define what is explicitly out of scope to prevent scope creep. Document exclusions clearly.


[COMMENT:
interviewing is done by the interviewing.md primitive

The confirmation should simply output the requirements and then get confirmation. It shouldn't be an interview.
]

### Output Definition (-> ?)

Execute full definition according to the output style reqding `transparency.md` to choose appropriate techniques, the default is a codeblock if not specified 

### Confirm with User (→ KR2)

Execute confirmation using `protocols/interviewing.md` (Understanding Validation) and `protocols/pragmatics.md` (Option Presentation) when definition is ready for review. Paraphrase understanding, present draft definition, verify accuracy, resolve ambiguities before finalizing.

---

## Additional Notes

**Requirements Engineering phases:** Elicitation → Analysis → Documentation → Validation → Management (IEEE 29148)

---

## References

- [IEEE 29148 - Requirements Specification](https://standards.ieee.org/ieee/830/1222/)
- [Acceptance Criteria Best Practices](https://www.productplan.com/glossary/acceptance-criteria/)
- [User Stories - Mountain Goat Software](https://www.mountaingoatsoftware.com/articles/advantages-of-user-stories-for-requirements)
- [Anthropic - Common Workflows](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
