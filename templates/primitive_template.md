---
name: primitive-name
description: >
  [What this primitive does. Include trigger keywords and phrases users naturally say.
  A primitive is an atomic cognitive action - it does one thing well.]
argument-hint: "[optional: e.g. [subject] or [topic]]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Primitive Name

**Goal:** [What this primitive achieves - one sentence.]

**Intent:** [Why this primitive exists - what problem it prevents.]

**Scope:** [What this primitive covers. State positively what IS included. A primitive answers one question that no other primitive answers.]

---

## Key Results

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track what has been done vs what remains
- `recovery.md` — Resume from where it was interrupted
- `reasoning.md` — Reason about approach before starting
- `[other].md` — [when/why this protocol applies]

---

## Preconditions

**Must be provided** (cannot proceed without — ask user):
- [condition]: [what to ask for]

**Self-satisfiable** (primitive will gather if not provided):
- [condition]: [how the primitive will satisfy it]

**Non-essential** (primitive works without, but enables more if present):
- [condition]: [what it enables]

---

## Postconditions

**Success:**
- [state that will be true when primitive completes successfully]

**Failure:**
- [state when primitive could not complete — what will be reported]

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

- **[action name]** — [what it does, when to use it]
- **[action name]** — [what it does, when to use it]
- **[action name]** — [what it does, when to use it]
- **[action name]** — [what it does, when to use it]

---

## Confirm

Before declaring done, verify against each key result:
- [question for KR1]
- [question for KR2]
- [question for KR3]

Report outcome explicitly: state whether the primitive succeeded or failed, and why.

---

## Use Cases

- [primary use case]
- [secondary use case]

---

## Tools

[List tools this primitive uses, or "None"]

## Hooks

[List hooks this primitive triggers, or "None"]
