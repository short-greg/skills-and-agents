---
name: feature-workflow
description: >
  Use when implementing a complete feature from requirements to validation.
  Triggers on: "build this feature", "implement this end-to-end", "full feature workflow".
argument-hint: "[feature to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, WebSearch, WebFetch, Task
protocols:
  - tracking
  - recovery
  - checklist_management
---

# Feature Workflow

**Goal:** Take a feature from initial idea to validated implementation.

**Intent:** Prevent incomplete or poorly-planned features by ensuring each stage (definition, design, implementation, validation) is completed before the next begins. Features ship with clear requirements, solid plans, and tested code.

**Scope:** End-to-end feature development. Includes: establishing requirements and success criteria, designing the technical approach, implementing the solution, and validating correctness. The workflow produces working, validated code that meets the defined requirements.

---

## Workflow Type

**Type:** Adaptive

**Style:** Imperative

**Uncertainty Handling:**
- If design surfaces unknowns (unclear dependencies, missing context), invoke `investigate` before finalizing design
- If design reveals scope is significantly larger than expected, stop and renegotiate with user before implementation
- If implementation reveals design gaps or ambiguities, surface to user rather than improvising
- If validation fails, invoke `investigate` to determine root cause, then loop back to appropriate step (define/design/implement)

**Iteration Handling:**
- Max 3 validation iterations per deliverable before escalating to user
- Each iteration must show measurable progress
- If same failure recurs, escalate immediately

---

## Steps

### Step 1: Orient

**Primitive:** orient

**Purpose:** Understand project structure, conventions, existing features, and where this feature fits

**Inputs:** Feature idea or request from user

**Outputs:**
- Project conventions identified (testing approach, documentation standards, code style)
- Existing relevant code identified (similar features, patterns to follow)
- Context for where feature fits in project architecture

**Self-satisfiable:** Reads project documentation, existing code, and conventions

### Step 2: Define Requirements

**Primitive:** define

**Purpose:** Establish what we're building and how we'll know it's done

**Inputs:**
- Feature idea (from user)
- Project context (from Step 1)

**Outputs:**
- Clear requirements (functional and non-functional)
- Success criteria (SMARB: Specific, Measurable, Achievable, Relevant, Bounded)
- Out-of-scope items (what this feature explicitly does NOT include)
- Acceptance criteria (how to verify the feature works)

### Step 3: Validate Definition

**Primitive:** validate

**Purpose:** Verify requirements are complete, unambiguous, and SMARB criteria are verifiable before investing in design

**Inputs:**
- Requirements from Step 2
- Success criteria from Step 2

**Outputs:**
- Pass/fail verdict
- Evidence (which criteria pass/fail)
- Gaps or ambiguities identified

**On failure:**
1. Invoke `investigate` to diagnose what's unclear or incomplete
2. Loop back to Step 2 with refinement context
3. Update checklist with specific items to address

**Gate:** User confirms definition is accurate and complete before proceeding to design

### Step 4: Design Approach

**Primitive:** design

**Purpose:** Figure out how to build it before writing code — architectural decisions, component design, test strategy

**Inputs:**
- Validated requirements from Steps 2-3
- Project conventions from Step 1

**Outputs:**
- Technical approach (architecture, technologies, patterns)
- Structural design (components, interfaces, data flow)
- Test strategy (what tests, at what levels, coverage targets)
- Implementation plan (rough task breakdown)

**Uncertainty triggers:**
- Unclear dependencies → invoke `investigate` to map dependencies
- Unknown API contracts → invoke `investigate` to research APIs
- Performance concerns → invoke `investigate` to profile or prototype

### Step 5: Validate Design

**Primitive:** validate

**Purpose:** Verify design addresses all requirements and is implementable before committing to code

**Inputs:**
- Design from Step 4
- Requirements from Step 2

**Outputs:**
- Pass/fail verdict
- Evidence (which design aspects satisfy which requirements)
- Design gaps or risks identified

**On failure:**
1. Invoke `investigate` to diagnose design issues
2. Determine if failure is:
   - Design gap → loop back to Step 4
   - Requirements ambiguity → loop back to Step 2
3. Update checklist with remediation tasks

**Alternative:** Use `critique` instead of `validate` for qualitative design assessment and improvement suggestions

**Gate:** User confirms design is acceptable before implementation

### Step 6: Implement

**Primitive:** implement

**Purpose:** Write the code following the validated design

**Inputs:**
- Validated design from Steps 4-5
- Requirements from Step 2
- Project conventions from Step 1

**Outputs:**
- Working code implementing the feature
- Tests (unit, integration, as specified in test strategy)
- Updated documentation (inline comments, README updates as needed)

**Implementation approach:**
- Follow test strategy from design
- Adhere to project conventions
- Maintain documentation proximity (update docs as code changes)

### Step 7: Validate Implementation

**Primitive:** validate

**Purpose:** Confirm implementation meets requirements and design before considering the feature complete

**Inputs:**
- Implementation from Step 6 (code + tests)
- Success criteria from Step 2
- Design from Step 4

**Outputs:**
- Pass/fail verdict with evidence
- Test results (coverage, pass rates)
- Requirement satisfaction analysis

**On failure:**
1. Invoke `investigate` to diagnose root cause
2. Determine loop-back point:
   - Implementation bug → loop back to Step 6
   - Design doesn't cover edge case → loop back to Step 4
   - Requirement was misunderstood → loop back to Step 2
3. Update checklist with specific fixes needed
4. Re-run from loop-back step (not from scratch — refine existing work)
5. Re-validate

**Iteration bounds:** Max 3 iterations before escalating to user

---

## Preconditions

**Must be provided:**
- feature description: what to build — ask user if not clear

**Self-satisfiable:**
- project context: read project docs (CLAUDE.md, .cursorrules, etc.) and existing code to understand conventions
- existing patterns: identify similar features to follow

---

## Postconditions

**Success:**
- Feature is fully implemented and validated
- All success criteria pass
- Code follows project conventions
- Tests meet coverage targets
- Documentation is updated

**Failure (blocked):**
- Requirements cannot be established (blocked at Step 2)
- Design reveals fundamental blockers or unknowns (blocked at Step 4)
- Implementation cannot satisfy requirements (blocked at Step 6)
- User aborts workflow

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`:
   - Identify last completed step from trace
   - Check if current step was interrupted mid-execution
   - Resume from appropriate point
3. Restore checklist state from trace

**Step-specific recovery notes:**

- **Step 1 (Orient):** Safe to re-run from beginning if interrupted
- **Step 2 (Define):** If partial requirements exist, read and continue from where left off
- **Step 4 (Design):** If partial design exists, read and refine rather than restart
- **Step 6 (Implement):** If interrupted mid-implementation:
  - Assess partial state (what's complete, what's in-progress, what's not started)
  - Review partial code for correctness before continuing
  - Complete in-progress items first, then move to not-started items
- **Step 7 (Validate):** Safe to re-run from beginning if interrupted

---

## On Validation Failure

**Validation failure is distinct from primitive failure** — the workflow completed the step but output doesn't meet criteria. This triggers iteration:

1. **Validate reports failure** — which criteria failed, with evidence
2. **Invoke investigate** — determine root cause and which step produced the defect:
   - "Implementation doesn't match design" → loop to Step 6
   - "Design doesn't cover edge case" → loop to Step 4
   - "Requirement was ambiguous" → loop to Step 2
3. **Update trace** — record iteration: "Validation failed, root cause: [X], looping to Step [N]"
4. **Add remediation tasks** — update checklist with specific fixes needed
5. **Re-run from loop-back step** — primitive reads prior output and refines (not starts fresh)
6. **Re-validate** — check if fixes resolved the issue

**Iteration bounds:**
- Default: max 3 iterations per deliverable before escalating to user
- If same failure recurs without progress, escalate immediately
- User can override bounds or abort at any iteration

---

## Quality Gates

This workflow includes quality gates after each deliverable:

| Step | Gate | Purpose |
|------|------|---------|
| Step 3 | Validate definition | Verify requirements before design |
| Step 5 | Validate design | Verify design before implementation |
| Step 7 | Validate implementation | Verify code meets requirements |

**User approval gates:**
- After Step 3: User confirms requirements are correct
- After Step 5: User confirms design is acceptable

These gates are **recommended** but optional — ask user if they want approval gates or automatic progression.

---

## Customization Points

When adapting this workflow for your project:

### Tool Configuration
- `${TOOL_CONFIG}` — location of AI tool configuration (e.g., `.claude/`, `.cursor/`)
- `${TASK_DIR}` — where trace files are stored
- `${SPEC_DIR}` — where specifications are stored (PRDs, plans, design docs)

### Testing Conventions
- Test framework (pytest, jest, rspec, etc.)
- Coverage targets (80%, 90%, etc.)
- Test file naming (test_*.py, *.test.js, *_spec.rb, etc.)
- Test organization (tests/ directory, inline, spec/, etc.)

### Documentation Standards
- Where feature docs live (README.md, docs/, inline comments)
- Documentation format (markdown, docstrings, JSDoc, etc.)
- What requires documentation (APIs, complex logic, configuration)

### Quality Standards
- Code style (linter configuration, formatting rules)
- Review requirements (self-review, peer review, automated checks)
- Performance requirements (benchmarks, profiling)

### Iteration Bounds
- Max validation iterations (default: 3)
- Escalation process (user notification, automatic abort)

---

## Example Usage

**User:** "Implement user authentication with email and password"

**Workflow execution:**

1. **Orient** — Read project, identify it's a web app using Node/Express, has auth middleware patterns
2. **Define** — Create requirements:
   - Functional: Email/password registration, login, logout, session management
   - Non-functional: Passwords hashed with bcrypt, sessions use secure cookies
   - Success criteria: Users can register, login, access protected routes, logout
   - Out of scope: OAuth, 2FA, password reset (can add later)
3. **Validate definition** — Verify requirements are SMARB and complete → PASS
4. **Design** — Technical approach:
   - User model with email/hashed password
   - Auth routes: POST /register, POST /login, POST /logout
   - Middleware for protected routes
   - Test strategy: Unit tests for User model, integration tests for auth flow
5. **Validate design** — Check design addresses all requirements → PASS
6. **Implement** — Write User model, auth routes, middleware, tests
7. **Validate implementation** — Run tests, verify all success criteria → PASS

**Outcome:** Feature complete, tested, validated.

---

## Best Practices

- **Don't skip steps** — Each step builds on prior steps; skipping causes rework
- **Validate early** — Catch issues in requirements/design before implementation
- **Use investigation** — When uncertain, investigate rather than guess
- **Iterate deliberately** — On validation failure, diagnose root cause before looping
- **Maintain trace** — Document progress for recovery and audit trail
- **Update docs as you go** — Don't defer documentation to the end

---

## Related Protocols

- **Required:**
  - [tracking.md](../protocols/tracking.md) — How to maintain trace file
  - [recovery.md](../protocols/recovery.md) — How to resume from interruption
  - [checklist_management.md](../protocols/checklist_management.md) — Dynamic checklist patterns

- **Optional:**
  - [manage_complexity_uncertainty_risk.md](../protocols/manage_complexity_uncertainty_risk.md) — Handle design uncertainty
  - [project_quality.md](../protocols/project_quality.md) — Assess implementation quality
  - [doc_maintenance.md](../protocols/doc_maintenance.md) — When to update documentation

---

## Related Primitives

This workflow composes these primitives in sequence:

1. [orient](../primitives/orient.md) — Understand context
2. [define](../primitives/define.md) — Establish requirements
3. [validate](../primitives/validate.md) — Verify deliverables
4. [design](../primitives/design.md) — Plan technical approach
5. [implement](../primitives/implement.md) — Write code
6. [investigate](../primitives/investigate.md) — Diagnose issues (on uncertainty or failure)
