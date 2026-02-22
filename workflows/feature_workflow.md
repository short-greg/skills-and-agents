---
name: feature-workflow
description: >
  Use when implementing a complete feature from requirements to validation.
  Triggers on: "build this feature", "implement this end-to-end", "full feature workflow".
argument-hint: "[feature to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, WebSearch, WebFetch, Task, TodoWrite
protocols:
  - tracking
  - recovery
  - checklist_management
  - reasoning_patterns
  - doc_maintenance
  - goals_and_objectives
  - manage_complexity_uncertainty_risk
---

# Feature Workflow

**Goal:** Take a feature from initial idea to validated implementation.

**Intent:** Prevent incomplete or poorly-planned features by ensuring requirements are clear, design is sound, and implementation is validated before completion.

**Scope:** End-to-end feature development: requirements, design, implementation, validation.

---

## Workflow Type

**Type:** Adaptive

**Style:** Hybrid (declarative goals with imperative validation gates)

---

## Key Results

1. Requirements are clear and SMARB (per `goals_and_objectives` protocol)
2. Design addresses all requirements
3. Implementation passes all tests and meets acceptance criteria
4. Documentation is updated (per `doc_maintenance` protocol)
5. Code follows project conventions

---

## Available Primitives

`orient`, `define`, `design`, `implement`, `validate`, `investigate`, `critique`, `brainstorm`

---

## Constraints

- Validate requirements before design
- Validate design before implementation
- Validate implementation before completion
- On uncertainty, invoke `investigate` before proceeding
- On validation failure, diagnose root cause before looping back

---

## Expert Reasoning (Required First)

Per `reasoning_patterns` protocol — before beginning, reason about:

1. **Complexity assessment** — Simple feature or complex? Uncertainty level?
2. **Risk identification** — What could go wrong? Where are unknowns?
3. **Approach selection** — Which primitives needed? What order?
4. **Alternatives** — What if primary approach fails?

Output reasoning before proceeding.

---

## Progress Tracking (Required)

Per `checklist_management` protocol — create and maintain a checklist throughout execution.

**On workflow start, create this checklist:**

```markdown
## Feature Progress

- [ ] 1. Understand context (orient)
- [ ] 2. Define requirements (define) — Gate: user confirms
- [ ] 3. Design approach (design) — Gate: user confirms
- [ ] 4. Implement feature (implement)
- [ ] 5. Validate implementation (validate)
- [ ] 6. Update documentation (doc_maintenance)
```

**Rules:**
- Display checklist at start of workflow
- Mark items `[x]` immediately after completing each phase
- Add sub-items dynamically when scope expands (e.g., "4a. Implement auth", "4b. Implement API")
- On validation failure, add remediation items and loop back
- Report progress after each completed item: "✅ [item] — [brief summary]"

---

## Execution

Execute by selecting and sequencing primitives to achieve key results. The following phases provide guidance, not rigid steps.

### Phase 1: Understand Context

**Primitive:** `orient` — See `primitives/orient.md`

Understand project structure, conventions, and where this feature fits.

### Phase 2: Define Requirements

**Primitive:** `define` — See `primitives/define.md`

Establish requirements and success criteria. Apply `goals_and_objectives` protocol for SMARB criteria.

**Gate:** Invoke `validate` on requirements. User confirms before proceeding.

### Phase 3: Design Approach

**Primitive:** `design` — See `primitives/design.md`

Plan technical approach.

**On uncertainty:** Per `manage_complexity_uncertainty_risk` protocol — invoke `investigate` before finalizing.

**Gate:** Invoke `validate` or `critique` on design. User confirms before proceeding.

### Phase 4: Implement

**Primitive:** `implement` — See `primitives/implement.md`

Write code following the design. Apply `doc_maintenance` protocol.

### Phase 5: Validate

**Primitive:** `validate` — See `primitives/validate.md`

Verify implementation meets requirements.

**On failure:** Per `checklist_management` protocol:
1. Invoke `investigate` to diagnose root cause
2. Determine loop-back point
3. Add remediation tasks to checklist
4. Re-execute and re-validate

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

Per `recovery` protocol — check for existing trace on startup, resume from last completed step.

---

## Iteration

Per `checklist_management` protocol:
- Max 3 validation iterations before escalating
- Each iteration must show progress
- If same failure recurs, escalate immediately

---

## Customization Points

**Prototype:** Simplified requirements, lighter validation, minimal docs.

**Production:** Full SMARB requirements, strict validation, documentation required.

**Library:** API compatibility, public API stability, changelog updates.
