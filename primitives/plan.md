---
name: plan
description: >
  Use when deciding what to do and in what order before starting work. Triggers on:
  "plan this", "how should we approach this", "what's the order of operations",
  "how do we tackle this", "break this down", "what should we do first",
  "create a plan", "sequence this", "what are the steps".
argument-hint: "[task or goal to plan]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Plan

**Goal:** Produce a clear sequence of high-level actions that will accomplish the goal, in the right order, with dependencies understood.

**Intent:** Prevent wasted effort from doing things in the wrong order, missing dependencies, or jumping into work without a strategy. A good plan makes execution straightforward.

**Scope:** Deciding what to do and in what order. Includes: identifying the high-level actions required to accomplish a goal, determining dependencies between actions, sequencing actions so prerequisites are met before dependents, identifying opportunities for parallelism, and surfacing unknowns that may require investigation before proceeding. Planning is about sequencing and ordering — it answers "what steps and in what order?" not "how do we implement each step?"

---

## Preconditions

**Must be provided:**
- goal or task: what needs to be accomplished — ask if not clear from context

**Self-satisfiable:**
- existing definition: if a definition or requirements doc exists, read it
- codebase context: read relevant files to understand current state and constraints

**Non-essential:**
- design decisions: if already made, will influence plan
- prior plan: if one exists, will be used as starting point or updated

---

## Postconditions

**Success:**
- A sequence of actions is established that will accomplish the goal
- Dependencies between actions are identified
- Parallelizable actions are noted where applicable
- The plan is at the right level of detail — actionable but not over-specified

**Failure:**
- Goal is too ambiguous to plan — further clarification of requirements is needed first
- Conflicting constraints make sequencing impossible without user input

---

## Key Results

1. Actions are complete — no obvious missing steps
2. Order is correct — no action requires something that hasn't been done yet
3. Dependencies are explicit
4. Parallelizable work is identified where relevant
5. The plan is at the right level of abstraction — not too granular, not too vague
6. User has confirmed the plan is sound

---

## Required Actions

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in this domain would approach planning
this task. Cover: their strategy, what they would do first and why, alternatives they would
consider, fallbacks if the primary approach fails, common mistakes to avoid, and what a
high-quality plan looks like for this type of task.
This is not a plan — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**

Before declaring done:
- Does the plan satisfy each key result?
- Has the user confirmed it is sound?
- Are there gaps or ordering issues?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Possible Actions

Select and sequence based on context and expert reasoning. Others may be used.

- **review definition**: read existing task definition, requirements, or goals — use to ground the plan in what's been agreed
- **identify actions**: enumerate the high-level actions needed to accomplish the goal
- **identify dependencies**: determine what must be done before what — critical for correctness
- **sequence actions**: order actions based on dependencies and logical flow
- **identify parallelism**: find actions that can run concurrently — useful for larger tasks
- **identify unknowns**: surface actions where approach is unclear and further research or technical design may be needed first
- **estimate scope**: rough sense of how much work each action involves — not deadlines, just relative size
- **draft plan**: write the plan in a clear, ordered format
- **review with user**: confirm the plan is complete and correctly ordered before proceeding

---

## Use Cases

- Before starting implementation to establish order of work
- Breaking a large feature into sequenced tasks
- Deciding what to tackle first when multiple things need doing
- Planning a refactor or migration where order matters

---

## Tools
None

## Hooks
None
