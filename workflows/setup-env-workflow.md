---
name: setup
description: >
  Consultative workflow to make a project AI-ready. Works with developer to understand their
  project, recommend approaches, and deliver a project-specific skill builder.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "set up AI environment", "make project AI-ready", "setup project".
argument-hint: "[project path or context]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup

**Goal:** Partner with developer to make their project AI-ready and deliver a project-specific skill builder.

**Intent:** Projects differ in structure, conventions, and needs. A prescriptive setup fails to fit. This workflow acts consultatively—understanding the project, presenting options, and creating customized deliverables.

**Scope:** Full consultation from assessment through delivery. Consultant recommends but developer decides.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach to project needs.

---

## Key Results - KR

1. Project's current state and AI-readiness gaps documented
2. Project-specific definition of "AI-ready" established with developer agreement
3. Implementation plan created and approved
4. Project made AI-ready with skill builder delivered encoding project conventions

## Requirements and Constraints - REQ

1. Track progress per `tracking_and_recovery.md` — use both TodoWrite and trace file
2. Ensure recoverable from interruption — check for existing trace on startup
3. Consultative not prescriptive — recommend with rationale, developer decides
4. Web research during consultation — current best practices for skill builders and setup
5. Iterate up to 3 times per task if developer requests changes before escalating

## Preconditions

**Required:** Access to project repository

**Optional:**
- Project context (what it does, tech stack)
- Team context (solo/team, AI tool experience)
- Current pain points with AI assistance
- Customization level preference
- Existing CLAUDE.md or conventions documentation

## Postconditions

**Success:** Assessment, definition, plan, and skill builder delivered; trace documents all decisions

**Failure:** Trace documents what was attempted and blockers; partial deliverables noted

---

## Steps

1. Create preliminary checklist → KR1-4, uses TodoWrite — formula: `setup - KR# - <task> - <details>`
2. Position consultation approach → KR2, uses `positioning.md` — output how you will consult
3. Orient to project → KR1, uses `orienting.md` — review all project documentation
4. Interview developer → KR1, KR2, uses `interviewing.md` — gather needs and preferences
5. Compose project-specific checklist → KR1-4 — update checklist with tasks for this project
6. Execute checklist tasks → KR3, KR4 — complete tasks, updating checklist as you go
7. Validate all KRs → KR1-4, uses `evaluating.md` — output evidence for each KR

---

## Tasks

**IMPORTANT:** Each task specifies modes to use. When executing a task you MUST read those modes if you haven't already. Select tasks based on context. Each task shows which KR it serves.

### Recover from Interruption (→ KR1, KR2, KR3, KR4)

**Goal:** Resume consultation with full context

**When:** Trace file exists from previous session

**Mode:** [orienting](../modes/orienting.md)

**Instructions:** Read trace, determine last completed action, present summary to developer, confirm how to proceed.

**Inputs:**
- Trace file (required)

**Outputs:**
- Recovery summary and confirmed resume point

---

### Assess and Gather Requirements (→ KR1, KR2)

**Goal:** Understand project state and developer needs

**When:** Starting consultation

**Mode:** [orienting](../modes/orienting.md), [investigating](../modes/investigating.md), [interviewing](../modes/interviewing.md)

**Instructions:** Orient to project structure and conventions. Investigate code for implicit patterns. Interview developer for context, pain points, customization preferences. Do web research on AI-readiness best practices. Document findings in trace.

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Outputs:**
- Assessment document with current state and gaps
- Requirements summary with developer confirmation

---

### Define AI-Readiness Criteria (→ KR2)

**Goal:** Establish what "AI-ready" means for this project

**When:** Assessment complete, ready to define scope

**Mode:** [defining](../modes/defining.md), [positioning](../modes/positioning.md)

**Instructions:** Present AI-readiness dimensions as options (see Notes). Recommend priorities based on assessment. Let developer choose focus areas. Document agreed criteria.

**Inputs:**
- Assessment document (required)
- Developer preferences (optional)

**Outputs:**
- Definition document with agreed criteria

---

### Plan Implementation (→ KR3)

**Goal:** Create roadmap for achieving AI-readiness

**When:** Definition complete, ready to plan

**Mode:** [planning](../modes/planning.md)

**Instructions:** Based on definition, plan deliverables. Do web research on current skill builder patterns. Present plan with options. Let developer approve or modify.

**Inputs:**
- Definition document (required)
- Constraints (optional)

**Outputs:**
- Approved implementation plan

---

### Design and Implement Skill Builder (→ KR4)

**Goal:** Design and create project-specific skill builder

**When:** Plan approved, ready to build

**Mode:** [designing](../modes/designing.md), [implementing](../modes/implementing.md)

**Instructions:** Follow [skill_builder_guideline.md](../guidelines/skill_builder_guideline.md) for options and template. Present options to developer based on project needs. Design skill builder encoding agreed conventions. Implement per approved design. Present drafts, iterate on feedback.

**Inputs:**
- Implementation plan (required)
- Project conventions (required)

**Outputs:**
- Skill builder design document
- Implemented skill builder
- Supporting documentation

---

### Validate Deliverables (→ KR1, KR2, KR3, KR4)

**Goal:** Verify all KRs met and hand off

**When:** Implementation complete

**Mode:** [evaluating](../modes/evaluating.md)

**Instructions:** Verify assessment reflects project state, definition criteria met, skill builder encodes conventions correctly. Present validation summary with evidence for each KR. Provide handoff with usage instructions.

**Inputs:**
- All deliverables (required)
- KR criteria (required)

**Outputs:**
- Validation summary with evidence for each KR
- Handoff documentation with usage instructions

---

### Set Up Project Environment (→ KR4)

**Goal:** Configure project structure, worktrees, and documentation for AI-readiness

**When:** Plan includes environment setup recommendations

**Mode:** [implementing](../modes/implementing.md)

**Instructions:** Based on plan, set up as needed:
- Create folders (tasks/, docs/) — adapt to existing structure
- Configure worktrees if developer wants parallel execution — consult best practices, present options
- Create/update CLAUDE.md files — root level and package level as needed

**Inputs:**
- Implementation plan (required)
- Project structure (required)
- Developer preferences (optional)

**Outputs:**
- Created folders with purpose explanation
- Worktree configuration if requested
- CLAUDE.md files at appropriate levels

---

## Additional Notes and Terms

**Skill Builder:** See [skill_builder_guideline.md](../guidelines/skill_builder_guideline.md) for options and template.

**AI-Readiness dimensions:** Understanding, navigation, behavior knowledge, task execution, skills, constraints, growth capability, maintainability.

**Customization Levels:** Minimal (basic CLAUDE.md), Moderate (project patterns), Comprehensive (full skill suite).

**Consultation vs Prescription:** Present options with recommendations. Developer decides.

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- Project-specific research conducted during consultation
