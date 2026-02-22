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

**Scope:** [Detailed description of what this skill covers. State positively what is included, not what is excluded. Be specific about the types of activities, artifacts, and concerns addressed.]

---

## Key Results

1. [measurable outcome]
2. [measurable outcome]
3. [measurable outcome]

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — [when/why this protocol applies to this skill]
- `recovery.md` — [when/why this protocol applies to this skill]
- `[other].md` — [when/why this protocol applies to this skill]

---

## Risks

[Risks inherent to this skill. What could go wrong? What requires careful handling?]

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | Low/Med/High | Low/Med/High | [How to prevent/handle] |
| [Risk 2] | Low/Med/High | Low/Med/High | [How to prevent/handle] |

---

## Preconditions

**Must be provided** (cannot proceed without — ask user):
- [condition]: [what to ask for]

**Self-satisfiable** (skill will gather if not provided):
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

## Tasks

Execute these tasks to achieve the key results. Select and sequence based on your reasoning. You may add tasks or skip tasks that don't apply.

### [Task Name]

[What this task does. Reference primitives or protocols as needed.]

### [Task Name]

[What this task does.]

### Confirm

Before declaring done, verify against each key result:
- Does the output satisfy each key result?
- If not, what is missing and what action addresses it?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Deliverables

[What this skill produces when successful.]

| Output | Location | Format |
|--------|----------|--------|
| [Output 1] | [path or description] | [format] |
| [Output 2] | [path or description] | [format] |

---

## Use Cases

- [primary use case]
- [secondary use case]

---

## Tools

[List tools this skill uses, or "None"]

## Hooks

[List hooks this skill triggers, or "None"]
