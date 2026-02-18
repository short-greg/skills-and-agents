---
name: skill-name
description: >
  [What this skill does. Include trigger keywords and phrases users naturally say.
  E.g. "Use when debugging, investigating why something broke, root cause analysis,
  something isn't working as expected."]
argument-hint: "[optional: e.g. [topic] or [filename]]"
disable-model-invocation: false  # true = user-invoked only (side effects, e.g. implement)
user-invocable: true             # false = Claude-only, hidden from / menu
allowed-tools: Read, Grep        # tools allowed without asking permission
# context: fork                  # uncomment to run in isolated subagent context
# agent: Explore                 # subagent type when context: fork
---

# Skill Name

**Goal:** [What this skill aims to achieve - one sentence.]

**Intent:** [Why this skill exists - what problem it solves.]

**Scope:** [What is and is not covered by this skill.]

---

## Preconditions

**Must be provided** (cannot proceed without, cannot self-satisfy — ask user):
- [condition]: [what to ask for]

**Self-satisfiable** (skill will gather at start if not provided):
- [condition]: [how the skill will satisfy it]

**Non-essential** (skill works without, but enables more if present):
- [condition]: [what it enables]

---

## Postconditions

**Success:**
- [state that will be true when skill completes successfully]

**Failure:**
- [state when skill could not complete — what will be reported]

---

## Key Results

This skill succeeds when:

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

---

## Required Actions

These must always be performed:

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert in this domain would approach this task.
Cover: their strategy, what they would do first and why, alternatives they would consider,
fallbacks if the primary approach fails, common mistakes to avoid, and what a high-quality
result looks like. This is not a plan — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**
Before declaring done, verify against each key result:
- Does the output satisfy each key result?
- If not, what is missing and what action addresses it?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Possible Actions

Select and sequence based on context and expert reasoning.
Others not listed may be used if appropriate.

- **[action]**: [what it does, when useful]
- **[action]**: [what it does, when useful]
- **[action]**: [what it does, when useful]

---

## Use Cases

- [primary use case]
- [secondary use case]

---

## Tools
None

## Hooks
None
