---
name: define
description: >
  Use when starting a new task, feature, or project and need to establish clear goals, requirements, and success criteria. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "define this", "what are the requirements", "let's figure out what we need to build", "I want to add X", "create a PRD", "what should we build", "clarify requirements", "scope this out".
argument-hint: "[task or feature to define]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Define

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring task is well-understood before any solution is attempted. Bad definitions lead to building wrong thing or never knowing when to stop.

**Scope:** Establishing what needs to be built and defining done. Includes clarifying goals and intent, capturing requirements and constraints, defining key terminology precisely enough to serve as evaluation gates, writing user stories that surface intended experience and edge cases, and specifying SMARB criteria — Specific, Measurable, Achievable, Relevant, and Bounded — to enable binary pass/fail validation. Definition answers "what are we building and how will we know it's done?" not "how will we build it?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Requirements and success criteria are complete (goal clear, key terms defined, user stories capture experience, SMARB criteria specified, out-of-scope stated)
2. User has confirmed definition is accurate

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Track which requirements captured vs remain per `tracking.md`
2. Write outcome-oriented SMARB success criteria per `goals_and_objectives.md`
3. Define key terms precisely enough to use as evaluation gates
4. Verify SMARB for each criterion: Specific? Measurable? Achievable? Relevant? Bounded?
5. Interview user to clarify intent, scope, constraints if ambiguous

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** None (primitive elicits requirements from user)

**Elicit if not provided:**
- Task understanding (interview user and/or review existing materials)
- Existing codebase context (read relevant files to understand current state)
- Existing documentation (read project docs, conventions)

**Optional:** Prior definition (if exists, read as starting point for refinement), technical constraints

## Postconditions

The resulting state after the primitive is finished.

**Success:** Task understood with goals, requirements, terminology, user stories, constraints, and key results established. Success criteria measurable and agreed upon. Output form is task-dependent (requirements doc, task description, PRD, or shared understanding).

**Failure:** Requirements could not be determined, conflicting requirements that cannot be resolved without further input.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Gather Requirements (→ KR1)

Request materials from user (transcripts, notes, prior specs, examples). Interview user with targeted questions to clarify intent, scope, constraints. Review materials (existing code, docs, specs).

Define terminology (produce precise definitions for key terms that become evaluation gates). Identify requirements (enumerate what must be true for task to be complete). Define user stories (describe who does what and why — surfaces intended experience and edge cases).

### Specify Success Criteria (→ KR1)

Identify constraints (what must NOT change, limitations on time/tech/compatibility). Specify key results (define measurable outcomes that confirm success). Verify SMARB for each criterion. Define evaluation criteria (specify how outputs will be judged). Identify out-of-scope (explicitly state what is not being addressed).

### Confirm with User (→ KR2)

Draft definition document. Review with user to confirm accuracy before finalizing. Verify no remaining ambiguities.

---

## Additional Notes and Terms

**SMARB Criteria:** Specific (unambiguous), Measurable (verifiable as pass/fail), Achievable (within scope), Relevant (addresses the goal), Bounded (clear stopping conditions).

**Use cases:** Starting new feature before implementation, clarifying ambiguous request before planning, creating PRD for larger work, establishing shared understanding between developer and AI.
