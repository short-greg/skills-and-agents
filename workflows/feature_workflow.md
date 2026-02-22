---
name: feature-workflow
description: >
  Use when implementing a complete feature from requirements to validation.
  Triggers on: "build this feature", "implement this end-to-end", "full feature workflow".
argument-hint: "[feature to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, WebSearch, WebFetch, Task, TodoWrite
---

# Feature Workflow

**Goal:** Take a feature from initial idea to validated implementation.

**Intent:** Prevent incomplete or poorly-planned features by ensuring requirements are clear, design is sound, and implementation is validated before completion.

**Scope:** End-to-end feature development: requirements, design, implementation, validation.

---

## Key Results

1. Uncertainties are resolved through reasoning and investigation prior to beginning work
2. Requirements are defined, clear, and outcome-oriented
3. Design addresses all requirements
4. Implementation passes all tests and meets acceptance criteria
5. Documentation is updated
6. Code follows project conventions

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track progress through all tasks. Report what you're doing as you work.
- `recovery.md` — On startup, check for existing progress. Resume from last completed task.
- `checklists.md` — Create a checklist after reasoning about the feature. Update it dynamically as scope changes.
- `reasoning.md` — Reason about approach BEFORE starting. Verify completion AFTER each task.
- `documentation.md` — Update documentation when the feature changes behavior or APIs.
- `goals_and_objectives.md` — Define outcome-oriented success criteria for the feature.
- `risk_management.md` — Identify risks and unknowns. Resolve them before implementation.

---

## Available Primitives

Primitives are atomic cognitive actions. They are in `skills/`. Use these to implement the feature. If you do not understand a primitive, read it before using it.

- `orient` — Understand project structure, conventions, and where this feature fits.
- `define` — Establish requirements and success criteria.
- `design` — Plan technical approach before implementing.
- `implement` — Write code following the design.
- `validate` — Verify work meets requirements with pass/fail verdicts.
- `investigate` — Reduce uncertainty by researching unknowns or diagnosing issues.
- `critique` — Review work for quality and identify improvements.
- `brainstorm` — Generate multiple distinct options when choices are needed.

You may use other operations when necessary, but prefer primitives when they fit.

---

## Constraints

- Validate requirements before design
- Validate design before implementation
- Validate implementation before completion
- On uncertainty, invoke `investigate` before proceeding
- On validation failure, diagnose root cause before looping back

---

## Tasks

Select and execute tasks to achieve each Key Result. Each task shows which KR it serves.

### Reason About Approach (→ KR1)

Per `reasoning.md` — reason about:

1. **Complexity assessment** — Simple feature or complex? Uncertainty level?
2. **Risk identification** — What could go wrong? Where are unknowns?
3. **Approach selection** — Which primitives needed? What order?
4. **Alternatives** — What if primary approach fails?

Output your reasoning.

### Understand Context (→ KR6)

Use `orient` primitive. Understand project structure, conventions, and where this feature fits.

### Define Requirements (→ KR2)

Use `define` primitive. Establish requirements and success criteria.

Per `goals_and_objectives.md` — requirements must be outcome-oriented and verifiable.

**Gate:** Validate requirements. Ask user to confirm before proceeding.

### Design Approach (→ KR3)

Use `design` primitive. Plan technical approach.

Per `risk_management.md` — if uncertainty exists, invoke `investigate` before finalizing design.

**Gate:** Validate or critique design. Ask user to confirm before proceeding.

### Implement (→ KR4, KR6)

Use `implement` primitive. Write code following the design.

Per `documentation.md` — update documentation when implementation changes behavior or APIs.

Per `checklists.md` — add sub-tasks dynamically when scope expands.

### Validate (→ KR4)

Use `validate` primitive. Verify implementation meets requirements.

**On failure:**
- Invoke `investigate` to diagnose root cause
- Determine which task to return to
- Add remediation tasks to checklist
- Re-execute and re-validate

### Update Documentation (→ KR5)

Per `documentation.md` — ensure all documentation reflects the implemented feature.

---

## Progress Tracking

Per `checklists.md` — build checklist using format: `<Skill> - KR<num> - <task>`

---

## Preconditions

**Must be provided:** Feature description (ask if unclear)

**Self-satisfiable:** Project context (read docs and code)

---

## Postconditions

**Success:** Feature implemented, validated, documented, following conventions.

**Failure:** Requirements cannot be established, design blocked, implementation cannot satisfy requirements, or user aborts.

---

## Recovery

Per `recovery.md` — check for existing trace on startup, resume from last completed task.

---

## Iteration

Per `checklists.md`:
- Max 3 validation iterations before escalating
- Each iteration must show progress
- If same failure recurs, escalate immediately

---

## Customization Points

**Prototype:** Simplified requirements, lighter validation, minimal docs.

**Production:** Full requirements, strict validation, documentation required.

**Library:** API compatibility, public API stability, changelog updates.
