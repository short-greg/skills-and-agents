# Create Workflow Protocol

This protocol defines how to create workflows that compose primitives into reliable multi-step processes.

---

## What is a Workflow

A workflow is a sequence of primitives orchestrated to accomplish a larger goal. Workflows are:

- **Composite**: They combine multiple primitives in sequence
- **Stateful**: They maintain progress via the tracking protocol
- **Recoverable**: They can resume from interruption via the recovery protocol
- **Goal-oriented**: They have a clear outcome that requires multiple cognitive steps

---

## When to Create a Workflow

Create a workflow when:

1. The task requires multiple distinct cognitive actions (primitives)
2. The order of actions matters — dependencies exist between steps
3. The task is common enough to warrant codification
4. Interruption recovery is valuable (long-running, multi-session)

Do NOT create a workflow when:

- A single primitive suffices
- The sequence is ad-hoc and unlikely to recur
- The steps have no dependencies (can be done in any order)

---

## Workflow vs. Primitive

| Aspect | Primitive | Workflow |
|--------|-----------|----------|
| Scope | One cognitive action | Multiple cognitive actions |
| State | Stateless (no trace) | Stateful (maintains trace) |
| Recovery | N/A | Via recovery protocol |
| Composition | Can be composed | Composes primitives |
| Duration | Single focused task | Extended multi-step process |

---

## Workflow Types

Workflows can be categorized along two dimensions:

### Dimension 1: Deterministic vs. Adaptive

**Deterministic Workflows:**
- Steps are known in advance and execute in fixed order
- No branching or conditional logic needed
- All steps are predictable, no uncertainty to resolve mid-workflow
- **Example:** "Format codebase" — orient → implement (apply formatters) → validate
- **When to use:** Routine tasks where the path is always the same

**Adaptive Workflows:**
- Steps may surface uncertainty, complexity, or blockers
- May need to invoke `investigate` before continuing
- May need to renegotiate scope with user
- **Example:** "Implement feature" — define → design → implement → validate, but design might surface unknowns requiring investigation
- **When to use:** Complex tasks where discovery is likely

### Dimension 2: Imperative vs. Declarative

**Imperative (Explicit Control Flow):**
- Workflow explicitly defines each step and the exact order
- "Do step A, then step B, then step C"
- Provides procedural control over execution
- **Example:**
  ```
  Step 1: Define requirements
  Step 2: Validate definition
  Step 3: Design approach
  Step 4: Validate design
  ...
  ```
- **When to use:** When the process must follow a specific sequence, when steps have strict dependencies, when explicit control is needed

**Declarative (Goal-Oriented):**
- Workflow defines the goal, constraints, and available primitives — execution figures out the path
- "Achieve goal G using primitives P1, P2, P3 with constraints C"
- Leverages primitive self-sufficiency and postconditions
- **Example:**
  ```
  Goal: Implement feature X with validated requirements and design
  Available primitives: define, design, implement, validate
  Constraints: Each deliverable must pass validation before next step
  Success criteria: Implementation passes all validation criteria
  ```
- **When to use:** When multiple valid paths exist, when you want flexibility in execution, when primitives can self-organize

**Combined approaches:**
- Most workflows are primarily imperative with declarative elements (e.g., "invoke design primitive" without specifying every action design takes)
- Our primitives enable this — they have imperative orchestration but declarative primitive invocation

### Declaring Workflow Type

When creating a workflow, explicitly state:

```markdown
## Workflow Type

**Type:** Deterministic | Adaptive
**Style:** Imperative | Declarative | Hybrid

**Assumptions** (for deterministic):
- [What makes this workflow predictable]

**Uncertainty Handling** (for adaptive):
- [What triggers investigation or scope renegotiation]
- [How the workflow handles discovered complexity]

**Execution Style** (if declarative or hybrid):
- [What goal is being achieved]
- [What primitives are available]
- [What constraints guide execution]
```

For adaptive workflows, consider:
- **Investigation triggers** — When should the workflow pause to investigate?
- **Scope renegotiation** — When should the workflow stop and consult the user?
- **Risk-first ordering** — Should high-uncertainty steps come early to fail fast?

**See also:** `protocols/manage_complexity_uncertainty_risk.md` for detailed techniques on identifying and mitigating complexity, uncertainty, and risk in workflows.

---

## Required Protocols

Every workflow MUST follow these protocols:

1. **Tracking Protocol** (`protocols/tracking.md`): Maintain a trace file throughout execution
2. **Recovery Protocol** (`protocols/recovery.md`): Check for and resume from prior execution on startup
3. **Checklist Management** (`protocols/checklist_management.md`): Dynamic checklist patterns for tracking progress, adding/removing items during execution, and loop-back iteration

Reference these protocols in the workflow definition.

## Optional Protocols

These protocols provide guidance for specific workflow challenges:

1. **Managing Complexity, Uncertainty, and Risk** (`protocols/manage_complexity_uncertainty_risk.md`): Techniques for identifying and mitigating complexity, uncertainty, and risk — especially useful for adaptive workflows
2. **Project Quality** (`protocols/project_quality.md`): Assessment framework for evaluating workflow quality across 6 dimensions (correctness, clarity, reliability, completeness, efficiency, testability)
3. **Modularity** (`protocols/modularity.md`): Principles for designing modular workflows with clean component boundaries and appropriate coupling
4. **Documentation Maintenance** (`protocols/doc_maintenance.md`): When and how to update workflow documentation to prevent drift

---

## Workflow Structure

### Frontmatter

```yaml
---
name: workflow-name
description: >
  Use when [situation requiring multi-step process]. Triggers on: "[phrase 1]", "[phrase 2]".
argument-hint: "[what argument is expected]"
disable-model-invocation: true  # workflows have side effects
user-invocable: true
allowed-tools: [all tools needed across all primitives]
protocols:
  - tracking
  - recovery
---
```

**Notes:**
- `disable-model-invocation: true` — workflows should be user-initiated due to scope
- `protocols` — explicitly list protocols this workflow follows
- `allowed-tools` — union of tools needed by all primitives in the workflow

### Goal, Intent, Scope

Same pattern as primitives, but scope describes the complete process:

```markdown
**Goal:** [What the complete workflow achieves — the end state.]

**Intent:** [Why this workflow exists — what problem the full process solves.]

**Scope:** [The complete process from start to finish. Describes what primitives are composed and what the workflow produces.]
```

### Steps

Workflows define steps, not actions. Each step invokes a primitive:

```markdown
## Steps

### Step 1: [Primitive Name]
**Primitive:** [primitive being invoked]
**Purpose:** [Why this step exists in the workflow]
**Inputs:** [What this step needs — from prior steps or user]
**Outputs:** [What this step produces for later steps]
**Gate:** [Optional: condition to proceed, or user approval point]

### Step 2: [Primitive Name]
...
```

**Notes:**
- Steps are sequential (dependencies are implicit in ordering)
- Each step maps to exactly one primitive
- Gates are optional checkpoints for user review

### Preconditions and Postconditions

```markdown
## Preconditions

**Must be provided:**
- [inputs needed to start the workflow]

**Self-satisfiable:**
- [context the workflow will gather at start]

## Postconditions

**Success:**
- [state when workflow completes successfully]

**Failure:**
- [conditions that cause workflow failure]
```

### Recovery Behavior

```markdown
## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules from `protocols/recovery.md`
3. Resume from the appropriate step

**Step-specific recovery notes:**
- Step N: [any special considerations for resuming this step]
```

---

## Writing Guidelines

### Defining Steps

Each step should:
- **Invoke exactly one primitive** — if a step needs multiple primitives, split it
- **Have clear inputs and outputs** — what flows between steps
- **Be skippable if already done** — trace enables this
- **Have a clear completion condition** — when is this step done

### Ordering Steps

Steps should be ordered such that:
- Prerequisites come before dependents
- Each step has what it needs from prior steps
- No step requires backtracking (if backtracking is needed, the workflow design is wrong)

### Gates

Gates are checkpoints between steps that verify quality before proceeding. There are two types:

**Quality Gates (validate/critique):**
- Verify each deliverable meets its success criteria before it becomes input to the next step
- Use `validate` for binary pass/fail against SMARB criteria
- Use `critique` for qualitative assessment and improvement feedback
- Pattern: `primitive → validate/critique → next primitive`
- Catches defects early when they're cheap to fix

**User Approval Gates:**
- Optional checkpoints where user reviews before high-stakes transitions
- Use when next step has significant consequences (e.g., implementation after design)
- Pattern: `**Gate:** User confirms [what] before proceeding to Step N+1`

**When to use quality gates:**
- After any step that produces a deliverable used by later steps
- After define (validate requirements before design)
- After design (validate/critique design before implementation)
- After implement (validate implementation meets requirements)

**Gate pattern:**
```markdown
### Step N: [Primitive]
**Primitive:** [primitive-name]
**Outputs:** [deliverable]

### Step N+1: Validate [Deliverable]
**Primitive:** validate
**Inputs:** [deliverable from Step N], success criteria
**Outputs:** Pass/fail verdict
**On failure:** Invoke investigate → loop back to Step N with refinement context
```

### Handling Primitive Failure

When a primitive fails, the workflow should:
1. Record the failure in the trace
2. Stop execution (do not proceed to the next step)
3. Surface the failure to the user with context
4. Wait for user guidance before retrying or aborting

### On Validation Failure

Validation failure is distinct from primitive failure — it means the workflow completed but the output doesn't meet success criteria. This triggers an iteration loop:

```
validate fails → investigate (diagnose) → loop back → refine → re-validate
```

**The Iteration Pattern:**

1. **Validate reports failure** — which criteria failed, with evidence
2. **Invoke investigate** — determine root cause and which step produced the defect:
   - "Implementation didn't match design" → loop to implement
   - "Design didn't cover this edge case" → loop to design
   - "Requirement was ambiguous" → loop to define
3. **Update trace** — record iteration: "Validation failed, root cause: [X], looping to Step [N]"
4. **Add remediation tasks** — update checklist with specific fixes needed
5. **Re-run from loop-back step** — primitive reads prior output and refines (not starts fresh)
6. **Re-validate** — check if fixes resolved the issue

**Iteration Bounds:**

Workflows should bound iterations to prevent infinite loops:
- Default: max 3 iterations before escalating to user
- Each iteration should make progress — if the same failure recurs, escalate immediately
- User can override bounds or abort at any iteration

**Declaring Iteration Handling:**

For adaptive workflows, include iteration handling in the workflow definition:

```markdown
## On Validation Failure

1. Invoke `investigate` to diagnose root cause
2. Determine loop-back point based on diagnosis
3. Update trace and checklist with remediation tasks
4. Re-run from loop-back step with refinement context
5. Re-validate

**Iteration bounds:** [max iterations before escalating]
**Escalation:** [what happens when bounds exceeded]
```

**Note:** The "no backtracking" guideline (in Ordering Steps) applies to the *design* of steps — steps should not be ordered such that they inherently require revisiting prior steps. Validation failure is different: it's a *runtime* discovery that prior work was insufficient, handled through the iteration pattern above.

---

## Example Workflow Structure

```markdown
---
name: feature-workflow
description: >
  Use when implementing a complete feature from requirements to validation.
  Triggers on: "build this feature", "implement this end-to-end", "full feature workflow".
argument-hint: "[feature to implement]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Write, Edit, Bash, WebSearch, WebFetch
protocols:
  - tracking
  - recovery
---

# Feature Workflow

**Goal:** Take a feature from initial idea to validated implementation.

**Intent:** Prevent incomplete or poorly-planned features by ensuring each stage (definition, design, implementation, validation) is completed before the next begins.

**Scope:** End-to-end feature development. Includes: establishing requirements and success criteria, designing the technical approach, implementing the solution, and validating correctness. The workflow produces working, validated code that meets the defined requirements.

---

## Steps

### Step 1: Define
**Primitive:** define
**Purpose:** Establish what we're building and how we'll know it's done
**Inputs:** Feature idea or request from user
**Outputs:** Requirements, success criteria (SMARB), out-of-scope items

### Step 2: Validate Definition
**Primitive:** validate
**Purpose:** Verify requirements are complete and SMARB criteria are verifiable
**Inputs:** Requirements from Step 1
**Outputs:** Pass/fail verdict
**On failure:** Invoke investigate → loop back to Step 1
**Gate:** User confirms definition is accurate before design

### Step 3: Design
**Primitive:** design
**Purpose:** Figure out how to build it before writing code
**Inputs:** Validated requirements from Step 1-2
**Outputs:** Technical approach, structural design, test strategy

### Step 4: Validate Design
**Primitive:** validate OR critique
**Purpose:** Verify design is complete and addresses requirements
**Inputs:** Design from Step 3, requirements from Step 1
**Outputs:** Pass/fail verdict (validate) or improvement recommendations (critique)
**On failure:** Invoke investigate → loop back to Step 3
**Gate:** User confirms design is acceptable before implementation

### Step 5: Implement
**Primitive:** implement
**Purpose:** Write the code
**Inputs:** Validated design from Step 3-4
**Outputs:** Working code, tests

### Step 6: Validate Implementation
**Primitive:** validate
**Purpose:** Confirm the implementation meets the requirements
**Inputs:** Implementation from Step 5, success criteria from Step 1
**Outputs:** Pass/fail verdict with evidence
**On failure:** Invoke investigate → loop back to appropriate step (Step 3 if design issue, Step 5 if implementation issue)

---

## Preconditions

**Must be provided:**
- feature description: what to build — ask if not clear

**Self-satisfiable:**
- project context: read CLAUDE.md, existing code to understand conventions

---

## Postconditions

**Success:**
- Feature is implemented and validated
- All success criteria pass
- Code follows project conventions

**Failure:**
- Requirements cannot be established (blocked at Step 1)
- Design reveals fundamental blockers (blocked at Step 2)
- Implementation fails to meet requirements (blocked at Step 4)

---

## Recovery

This workflow follows the recovery protocol. On startup:

1. Check for existing trace at `${TASK_DIR}/trace.md`
2. If found, apply recovery rules
3. Resume from the appropriate step

**Step-specific recovery notes:**
- Step 3 (Implement): If interrupted mid-implementation, assess partial state before continuing
- Step 4 (Validate): Can safely re-run from beginning if interrupted

---

## Workflow Type

**Type:** Adaptive

**Uncertainty Handling:**
- If design surfaces unknowns, invoke `investigate` before finalizing design
- If design reveals scope is larger than expected, stop and renegotiate with user before implementation
- If implementation reveals design gaps, surface to user rather than improvising
```

---

## Guidelines Summary

### Required

These must be present in every workflow:

- Workflow type declared (deterministic or adaptive)
- Tracking protocol followed
- Recovery protocol followed
- Steps map to exactly one primitive each
- Inputs/outputs defined for each step
- Postconditions are verifiable

### Recommended

Include these unless clearly not applicable:

- Quality gates after each deliverable (validate/critique each output before it becomes input to next step)
- User approval gates at high-stakes transitions (before implementation, before destructive actions)
- Step-specific recovery notes for complex steps
- Uncertainty handling documented (for adaptive workflows)
- Risk-first ordering for adaptive workflows (tackle unknowns early)

### Optional

Consider these during workflow definition. If unclear, ask the user. If already obvious from context, skip.

- **Parallelism** — Can any steps run concurrently?
- **Iteration bounds** — Max loops on validation failure before escalating (default: 3)
- **Rollback/Compensation** — Should partial work be cleaned up on failure? What compensating actions undo completed steps?
- **Retry semantics** — Which steps are idempotent and can be safely retried? How many retries? Exponential backoff?
- **Data flow** — How does data flow between steps? Are there explicit data dependencies beyond control flow?
- **Resource constraints** — Who/what executes each step (human vs. AI)? Any resource limitations?
- **Data persistence** — Where are step outputs stored? (trace, separate files, memory)
- **Observability** — Are progress indicators needed beyond the trace?

### Workflow Composition

**Current limitation:** Sub-workflows (workflows invoking other workflows) are not reliable due to context management. Flatten sub-workflows into the parent workflow's steps instead.

---

## Workflow Patterns from Research

These patterns come from established workflow research (van der Aalst, Temporal/Cadence, BPM). Use them as needed — not all workflows require all patterns.

### Control Flow Patterns

**Basic patterns:**
- **Sequence**: Steps execute in order (most common)
- **Parallel Split**: Multiple steps execute concurrently
- **Synchronization**: Wait for parallel branches to complete before continuing
- **Exclusive Choice**: Conditional branching based on data or state
- **Deferred Choice**: Environment determines which path to take (e.g., user action)

**When to use:** Most workflows are simple sequences. Use parallel split when steps are independent. Use choice patterns when workflow paths diverge based on conditions.

### Compensation Pattern (Saga)

When a workflow fails partway through, compensation undoes completed steps:
- Each step has a compensating action (the "undo")
- On failure, execute compensations in reverse order
- Useful for workflows that modify external state (databases, APIs, file systems)

**Example:**
```
define → validate → design → [design fails validation]
Compensation: None needed (documents don't require cleanup)

deploy-database → run-migration → update-config → [config update fails]
Compensation: rollback-config → revert-migration → teardown-database
```

**When to use:** Workflows with side effects that need to be undone on failure. Not needed for read-only or idempotent workflows.

### Retry & Fault Tolerance

**Retry strategies:**
- **Immediate retry**: Retry instantly (useful for transient network errors)
- **Exponential backoff**: Increasing delays between retries (1s, 2s, 4s, 8s...)
- **Bounded retries**: Max retry count before failing
- **Idempotency requirement**: Step must be safely retryable without side effects

**When to use:** Steps that call external services, I/O operations, or anything with transient failures. Ensure steps are idempotent before enabling retries.

### Data Flow Patterns

**Explicit data dependencies:**
- **Data transformation**: Step N transforms data for Step N+1
- **Data aggregation**: Step N+1 requires outputs from multiple prior steps
- **Data branching**: One output feeds multiple downstream steps

**When to use:** When data dependencies are complex or non-obvious. Most workflows have implicit data flow through step inputs/outputs.

### Resource Patterns

**Who/what executes each step:**
- **Human-in-the-loop**: Step requires user input or approval
- **AI-only**: Step is fully automated
- **Delegation**: Step is assigned to a specific resource (person, service, agent)

**When to use:** When execution depends on specific resources or requires human oversight.

### Checkpoint & Recovery

**State persistence:**
- **After each step**: Persist state so workflow can resume from any point (our tracking protocol does this)
- **At key milestones**: Persist only at critical points (lighter weight)
- **Optimistic**: Assume success, persist only on request

**When to use:** Long-running workflows benefit from full checkpointing. Short workflows may not need it.

---

## Workflow Definition Questionnaire

When creating a workflow, confirm decisions in a code block. Skip items that are already clear from context. Ask the user if uncertain.

```markdown
## Workflow Confirmation

### Required
- [ ] Workflow type: [Deterministic / Adaptive]
- [ ] Workflow style: [Imperative / Declarative / Hybrid]
- [ ] Steps identified: [list primitives in order (imperative)] OR [goal + available primitives (declarative)]
- [ ] Inputs/outputs per step: [defined / need clarification]

### Recommended
- [ ] Quality gates: [validate/critique after each deliverable]
- [ ] User approval gates: [where, if any]
- [ ] Uncertainty handling (if adaptive): [triggers for investigation/renegotiation]

### Optional (if applicable)
- [ ] Parallelism: [which steps, if any]
- [ ] Iteration bounds: [max loops on validation failure, or N/A if deterministic]
- [ ] Compensation/Rollback: [compensating actions for completed steps, if side effects exist]
- [ ] Retry semantics: [which steps, retry strategy, idempotency requirements]
- [ ] Data flow: [explicit data dependencies beyond control flow]
- [ ] Resource constraints: [human vs. AI execution, resource assignments]
- [ ] Data persistence: [where outputs are stored]
- [ ] Observability: [progress indicators needed]
```

---

## Quality Checklist

Before finalizing a workflow, verify:

- [ ] **Workflow type declared**: Explicitly stated as deterministic or adaptive
- [ ] **Workflow style declared**: Explicitly stated as imperative, declarative, or hybrid
- [ ] **Protocols referenced**: Workflow lists tracking and recovery protocols
- [ ] **Steps map to primitives**: Each step invokes exactly one primitive
- [ ] **Inputs/outputs defined**: Clear data flow between steps
- [ ] **Order is correct**: No step requires output from a later step
- [ ] **Quality gates present**: Each deliverable is validated/critiqued before becoming input to next step
- [ ] **User approval gates appropriate**: User checkpoints where valuable
- [ ] **Recovery addressed**: Step-specific recovery notes where needed
- [ ] **Failure handling clear**: What happens when a step fails
- [ ] **Postconditions are verifiable**: Can determine if workflow succeeded
- [ ] **Uncertainty handling** (adaptive only): Clear triggers for investigation or scope renegotiation
- [ ] **Iteration handling** (adaptive only): Validation failure triggers investigate → loop-back → refine pattern
- [ ] **Optional items addressed**: Either included, explicitly skipped, or confirmed with user

---

## Common Mistakes

- **Undeclared workflow type**: Always state whether deterministic or adaptive
- **Undeclared workflow style**: Always state whether imperative, declarative, or hybrid
- **Steps that aren't primitives**: Each step should map to exactly one primitive
- **Missing protocol references**: Workflows must reference tracking and recovery
- **Unclear data flow**: Inputs and outputs between steps must be explicit
- **No quality gates**: Each deliverable should be validated/critiqued before it becomes input to the next step — catches defects early
- **No user approval gates**: Long workflows benefit from user checkpoints at high-stakes transitions
- **Ignoring recovery**: Workflows will be interrupted; plan for it
- **Too many steps**: If a workflow has >6-7 steps, consider splitting into sub-workflows
- **Backtracking**: If steps require revisiting prior steps, redesign the workflow
- **Adaptive workflow without uncertainty handling**: If adaptive, must define what triggers investigation or renegotiation
- **Adaptive workflow without iteration handling**: If workflow includes validation, must define what happens on validation failure — investigate → loop-back → refine pattern
