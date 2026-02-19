---
name: define
description: >
  Use when starting a new task, feature, or project and need to establish clear
  goals, requirements, and success criteria. Triggers on: "define this", "what are
  the requirements", "let's figure out what we need to build", "I want to add X",
  "create a PRD", "what should we build", "clarify requirements", "scope this out".
argument-hint: "[task or feature to define]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Define

**Goal:** Establish when work is done — what needs to be built, what success looks like, and how completion will be verified.

**Intent:** Prevent wasted effort by ensuring the task is well-understood before any solution is attempted. Bad definitions lead to building the wrong thing or never knowing when to stop.

**Scope:** Establishing what needs to be built and defining done. Includes: clarifying goals and intent, capturing requirements and constraints, defining key terminology precisely enough to serve as evaluation gates, writing user stories that surface intended experience and edge cases, and specifying SMARB criteria — Specific, Measurable, Achievable, Relevant, and Bounded — to enable binary pass/fail validation.

---

## Preconditions

**Self-satisfiable:**
- task understanding: interview user and/or review existing materials to surface what needs to be built
- existing codebase context: read relevant files to understand current state
- existing documentation: read project docs, conventions

**Non-essential:**
- prior spec: if one exists it will be used as a starting point
- technical constraints: if known, will shape requirements

---

## Postconditions

**Success:**
- The task is understood: goals, requirements, terminology, user stories, constraints, and key results are established
- All key terms are precisely defined and usable as evaluation gates
- Success criteria are measurable and agreed upon
- Output form is task-dependent: may be a formal requirements document, a simple task description, a PRD, or shared understanding captured in conversation

**Failure:**
- Requirements could not be determined (e.g. user is unable to clarify what they want)
- Conflicting requirements that cannot be resolved without further input

---

## Key Results

1. Goal is stated clearly and unambiguously
2. Requirements are complete — no obvious gaps
3. User stories capture the intended experience
4. Key terms are defined precisely enough to use as evaluation gates
5. Success criteria are SMARB — Specific (unambiguous), Measurable (verifiable as pass/fail), Achievable (within scope), Relevant (addresses the goal), Bounded (clear stopping conditions)
6. Out-of-scope items are explicitly stated
7. User has confirmed the definition is accurate

---

## Required Actions

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in requirements and product definition
would approach this task. Cover: their strategy, what they would do first and why,
alternatives they would consider, fallbacks if the primary approach fails, common mistakes
to avoid, and what a high-quality definition looks like for this type of task.
This is not a plan — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**

Before declaring done:
- Does the definition satisfy each key result?
- Has the user confirmed it is accurate?
- Are there any remaining ambiguities?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Possible Actions

Select and sequence based on context and expert reasoning. Others may be used.

- **request materials**: ask user for relevant inputs — transcripts, notes, prior specs, examples — use when context is thin or task is complex
- **interview user**: ask targeted questions to clarify intent, scope, constraints — use when requirements are unclear or underspecified
- **review materials**: read existing code, docs, specs — use when context exists that shapes requirements
- **define terminology**: produce precise definitions for key terms — always useful, terms become evaluation gates
- **identify requirements**: enumerate what must be true for the task to be complete
- **define user stories**: describe who does what and why — surfaces intended experience and edge cases
- **identify constraints**: what must NOT change, what limitations exist (time, tech, compatibility)
- **specify key results**: define measurable outcomes that confirm success
- **verify SMARB**: for each criterion, check: Is it Specific (unambiguous)? Measurable (pass/fail)? Achievable? Relevant? Bounded (clear stopping condition)? Flag and refine any that fail
- **define evaluation criteria**: specify how outputs will be judged — used as gates in later review stages
- **identify out-of-scope**: explicitly state what is not being addressed
- **identify risks**: business risks — market, user adoption, stakeholder alignment — not technical implementation risks
- **draft definition**: write the definition document
- **review with user**: confirm the definition is accurate before finalizing

---

## Use Cases

- Starting a new feature before any implementation
- Clarifying an ambiguous request before planning
- Creating a PRD for a larger piece of work
- Establishing shared understanding between developer and AI before proceeding

---

## Tools
None

## Hooks
None
