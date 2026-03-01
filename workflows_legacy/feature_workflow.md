**This template inherits from [../templates/skill_template.md](../templates/skill_template.md) with workflow-specific additions:**
- **Steps section** — Sequential execution order
- **Tasks section** — Replaces generic Execution Items
- **Available Primitives section** — Lists primitives used in this workflow
- **Recovery requirement** — Standard workflow requirements include progress tracking, recovery, iteration

---
name: feature-workflow
description: >
  Use when implementing a complete feature from requirements to validation.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow. They are specified in the instruction body.
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

## Key Results - KR

You must satisfy these to complete the skill successfully.

1. Requirements are defined, clear, and outcome-oriented
2. Design addresses all requirements
3. Implementation passes all tests and meets acceptance criteria
4. Documentation is updated and code follows project conventions

## Requirements and Constraints - REQ

Constraints on how to complete the skill.

1. Progress tracked per `tracking.md` — preliminary checklist created before starting work
2. Recoverable from interruption per `tracking.md` and `recovery.md` — check for existing trace on startup, resume from last completed task
3. Validate requirements before design
4. Validate design before implementation
5. Validate implementation before completion
6. On uncertainty, invoke `investigate` before proceeding
7. On validation failure, diagnose root cause before looping back
8. Per `documentation.md` — update documentation when feature changes behavior or APIs
9. Per `risk_management.md` — identify and resolve risks/unknowns before implementation
10. Iterate up to 3 times if validation fails, diagnose root cause, add remediation tasks, re-execute and re-validate

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Feature description

**Elicit if not provided:**
- Project context (read docs and code)
- Conventions (from codebase)

**Optional:** None

## Postconditions

The resulting state after the skill is finished.

**Success:** Feature implemented, validated, documented, following conventions.

**Failure:** Requirements cannot be established, design blocked, implementation cannot satisfy requirements, or user aborts.

## Steps

Complete the Tasks in this order.

Steps:
1. Reason about approach
2. Understand context (orient)
3. Define requirements
4. Design approach
5. Implement feature
6. Validate implementation
7. Update documentation

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

### Understand Context (→ KR4)

Use `orient` primitive. Understand project structure, conventions, and where this feature fits.

### Define Requirements (→ KR1)

Use `define` primitive. Establish requirements and success criteria.

Per `goals_and_objectives.md` — requirements must be outcome-oriented and verifiable.

**Gate:** Validate requirements. Ask user to confirm before proceeding.

### Design Approach (→ KR2)

Use `design` primitive. Plan technical approach.

Per `risk_management.md` — if uncertainty exists, invoke `investigate` before finalizing design.

**Gate:** Validate or critique design. Ask user to confirm before proceeding.

### Implement (→ KR3, KR4)

Use `implement` primitive. Write code following the design.

Per `documentation.md` — update documentation when implementation changes behavior or APIs.

Per `tracking.md` — add sub-tasks dynamically when scope expands.

### Validate (→ KR3)

Use `validate` primitive. Verify implementation meets requirements.

**On failure:**
- Invoke `investigate` to diagnose root cause
- Determine which task to return to
- Add remediation tasks to checklist
- Re-execute and re-validate

### Update Documentation (→ KR4)

Per `documentation.md` — ensure all documentation reflects the implemented feature.

---

## Available Primitives

Primitives are atomic cognitive actions in `primitives/`. Use these to execute the workflow. If you do not understand a primitive, read it before using it.

- `orient` — Understand project structure, conventions, and where this feature fits
- `define` — Establish requirements and success criteria
- `design` — Plan technical approach before implementing
- `implement` — Write code following the design
- `validate` — Verify work meets requirements with pass/fail verdicts
- `investigate` — Reduce uncertainty by researching unknowns or diagnosing issues
- `critique` — Review work for quality and identify improvements
- `brainstorm` — Generate multiple distinct options when choices are needed

---

## Validation Criteria

- [ ] **Structure:** All sections present with one-line imperatives. Frontmatter complete.
- [ ] **KRs vs Requirements:** KRs are outcomes (WHAT), Requirements are constraints (HOW), no overlap
- [ ] **Traceability:** Tasks show (→ KR#), all KRs served by at least one task
- [ ] **Preconditions:** Categorized as Required, Elicit if not provided, or Optional
- [ ] **No redundancy:** Each piece of information appears exactly once
- [ ] **Recovery:** Includes progress tracking, recovery behavior, iteration limit in Requirements
- [ ] **Coherent:** Steps flow logically, no contradictions between sections
- [ ] **Concise:** As few words as possible, no duplication
- [ ] **Complete:** All necessary information provided, all KRs achievable from Tasks
- [ ] **Precise:** Specific, unambiguous language, clear definitions

---

## Additional Notes and Terms

**Customization Points:**

- **Prototype:** Simplified requirements, lighter validation, minimal docs
- **Production:** Full requirements, strict validation, documentation required
- **Library:** API compatibility, public API stability, changelog updates
