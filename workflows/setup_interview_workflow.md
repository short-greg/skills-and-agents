---
name: setup-interview
description: >
  Consultative workflow to understand a project and plan AI-readiness setup.
  Interviews developer, establishes interaction mode, defines criteria, outputs implementation plan.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "set up AI environment", "make project AI-ready", "setup project".
argument-hint: "[project path or context]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup Interview

**Goal:** Understand project and team needs, establish interaction mode, define AI-readiness criteria, output approved implementation plan.

**Intent:** Projects differ in structure, conventions, and needs. Before implementing anything, understand what the developer wants and how they want to collaborate with AI.

**Scope:** Consultation from assessment through plan approval. No implementation — that's handled by setup-environment and setup-skills workflows.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach to project needs.

---

## Key Results - KR

1. Interaction Mode established (Lead/Senior/Peer/Junior) and documented
2. Project's current state and AI-readiness gaps documented
3. Project-specific definition of "AI-ready" agreed with developer
4. Implementation plan approved and task.md created with all decisions

## Requirements and Constraints - REQ

1. Track progress per `tracking_and_recovery.md` — use both TodoWrite and trace file
2. Ensure recoverable from interruption — check for existing trace on startup
3. Consultative not prescriptive — recommend with rationale, developer decides
4. Establish Interaction Mode early — it determines how rest of consultation proceeds
5. Iterate up to 3 times per task if developer requests changes before escalating

## Preconditions

**Required:** Access to project repository

**Optional:**
- Project context (what it does, tech stack)
- Team context (solo/team, AI tool experience)
- Current pain points with AI assistance
- Existing CLAUDE.md or conventions documentation

## Postconditions

**Success:**
- task.md created at `tasks/{TASK_ID}/task.md` documenting all decisions
- Interaction mode, assessment, definition, and plan all documented
- setup-environment and setup-skills workflows unlocked (moved to `.claude/skills/`)

**Failure:** Trace documents what was attempted and blockers; partial deliverables noted

---

## Steps

1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with formula: `interview - KR# - <task> - <details>`
2. Check for existing trace → Execute "Recover from Interruption" task if trace exists
3. Assess and gather requirements → KR1, KR2. Execute "Assess and Gather Requirements" task. You MUST establish Interaction Mode during this step
4. Define AI-readiness criteria → KR3. Execute "Define AI-Readiness Criteria" task
5. Plan implementation → KR4. Execute "Plan Implementation" task. Output action plan for approval
6. Create task.md → KR4. Write all decisions to `tasks/{TASK_ID}/task.md`
7. Unlock next workflows → Move setup-environment and setup-skills to `.claude/skills/`
8. Hand off → Inform user to invoke `/setup-environment` next

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.
3. When executing a Task:
   1. You MUST Read the mode to understand typical actions
   2. You MUST choose actions based on the Instructions and the Mode.

### Recover from Interruption (→ KR1, KR2, KR3, KR4)

**Goal:** Resume consultation with full context

**When:** Trace file exists from previous session

**Mode:** [orienting](../modes/orienting.md)

**Instructions:**
1. Review the trace
2. Recover your position

**Inputs:**
- Trace file (required)

**Outputs:**
- Recovery summary and confirmed resume point

---

### Assess and Gather Requirements (→ KR1, KR2)

**Goal:** Understand project state, developer needs, and establish how developer wants to work with AI

**When:** Starting consultation

**Mode:** [interviewing](../modes/interviewing.md)

**Instructions:**
1. Output Position Statement as a consultant
2. Orient to project (read existing docs, structure)
3. **Establish Interaction Mode first** — ask how developer wants to collaborate with AI
4. Based on Interaction Mode, gather remaining requirements appropriately
5. Output assessment, then ask questions arising from that assessment

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Outputs:**
- Interaction Mode (Lead/Senior/Peer/Junior) documented
- Assessment document with current state and gaps
- Requirements summary with developer confirmation

**Interaction Modes:**
- **Lead:** AI is almost entirely autonomous, developer expects good output without much review
- **Senior:** AI is fairly autonomous, receives some feedback, less collaboration than Peer
- **Peer:** AI and developer collaborate heavily on designs and plans
- **Junior:** Developer heavily reviews AI's output, confirms designs and plans, AI does not jump into implementation

---

### Define AI-Readiness Criteria (→ KR3)

**Goal:** Establish what "AI-ready" means for this project

**When:** Assessment complete, Interaction Mode established

**Mode:** [defining](../modes/defining.md)

**Instructions:**
1. Present AI-readiness dimensions as options (see Additional Notes)
2. Recommend priorities based on assessment
3. Let developer choose focus areas
4. Document agreed criteria

**Inputs:**
- Assessment document (required)
- Interaction Mode (required)

**Outputs:**
- Definition document with agreed criteria

---

### Plan Implementation (→ KR4)

**Goal:** Create roadmap for achieving AI-readiness

**When:** Definition complete

**Mode:** [planning](../modes/planning.md)

**Instructions:**
1. Propose implementation plan based on definition and Interaction Mode
2. Plan should specify which tasks from setup-environment and setup-skills to execute
3. Get approval before finalizing

**Inputs:**
- Definition document (required)
- Interaction Mode (required)
- Constraints (optional)

**Outputs:**
- Approved implementation plan specifying:
  - Environment setup tasks (for setup-environment workflow)
  - Skills setup tasks (for setup-skills workflow)

---

## Additional Notes and Terms

**AI-Readiness Dimensions:** Understanding, navigation, behavior knowledge, task execution, skills, constraints, growth capability, maintainability.

**Customization Levels:**
- Minimal: Basic CLAUDE.md with project overview
- Moderate: CLAUDE.md plus project patterns and conventions
- Comprehensive: Full skill suite with skill builder and custom workflows

**Task Folder:** Recommended: `./tasks/{TASK_ID}/` with `./tasks/{TASK_ID}/local_context/` for task-specific context.

**Docs Folder:** Recommended: `./docs/`

**task.md Contents:** Must include:
- Interaction Mode selected
- Assessment summary
- AI-readiness criteria agreed
- Implementation plan
- Any constraints or preferences discovered

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
