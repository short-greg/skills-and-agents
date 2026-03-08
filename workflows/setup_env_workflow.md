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
8. Inform the user the next step is to setup up the project with the `/setup_env_workflow` after refreshing Claude.

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

### Create Skill Builder Skill (→ KR4)

**Goal:** Create a project-specific skill builder that can generate skills following project conventions

**When:** Project conventions understood, team preferences gathered

**Mode:** [designing](../modes/designing.md), [implementing](../modes/implementing.md)

**Instructions:** Follow [skill-builder-guideline.md](../guidelines/skill-builder-guideline.md). Research current best practices. Output expert reasoning on how to approach this project's skill builder. Design skill builder that accounts for: project setup/codebase, team workflow style preferences (imperative/declarative, prescriptive/descriptive), consultation orientedness, naming conventions. Present recommendations for team review. Implement based on approved design.

**Inputs:**
- Project conventions (required)
- Team preferences (required)
- Assessment document (required)

**Outputs:**
- Skill builder skill customized to project
- Documentation of how the skill builder works

---

### Set Up Worktree Environment (→ KR4)

**Goal:** Determine if worktrees are appropriate and configure them if needed

**When:** Team has parallel work needs or requests worktree setup

**Mode:** [interviewing](../modes/interviewing.md), [implementing](../modes/implementing.md)

**Instructions:** Follow [worktree-setup-guideline.md](../guidelines/worktree-setup-guideline.md). Assess whether team benefits from worktrees. If so, discover preferences for: directory structure, environment sync needs, shell integration. If orchestration workflows are needed, create worktree-orchestrate and worktree-task workflows per guideline. Present options, let team decide.

**Inputs:**
- Team's parallel work patterns (required)
- Project structure (required)

**Outputs:**
- Decision on whether worktrees are appropriate (documented)
- If appropriate: worktree configuration, shell functions, .worktreesync
- If orchestration needed: orchestrate and task workflows

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

**Folder structure:**
- Create tasks/ folder if needed — for task specs, parallel work coordination
- Create docs/ folder if needed — for project documentation, specs, architecture decisions
- Adapt to existing structure — don't impose if project has conventions

**CLAUDE.md files:**
- Root level: project overview, codebase navigation, key conventions, available skills
- Package level (if monorepo): package-specific context, local patterns
- Emphasize: how AI should navigate codebase, important patterns/anti-patterns, what skills exist and how to invoke them

**Worktree setup (if requested):**
- Configure directory structure per worktree-setup-guideline
- Install shell functions if wanted
- Create .worktreesync with environment files to sync
- If orchestration needed: install worktree-orchestrate and worktree-task as skills in .claude/skills/

**Inputs:**
- Implementation plan (required)
- Project structure (required)
- Developer preferences (optional)

**Outputs:**
- Created folders with purpose explanation
- CLAUDE.md files documenting project conventions and available skills
- Worktree configuration if requested
- Orchestration workflows installed as skills if needed

---

## Additional Notes and Terms

**Skill Builder:** See [skill-builder-guideline.md](../guidelines/skill-builder-guideline.md) for options and template.

**Worktree Setup:** See [worktree-setup-guideline.md](../guidelines/worktree-setup-guideline.md) for worktree and orchestration workflow guidance.

**AI-Readiness dimensions:** Understanding, navigation, behavior knowledge, task execution, skills, constraints, growth capability, maintainability.

**Customization Levels:** Minimal (basic CLAUDE.md), Moderate (project patterns), Comprehensive (full skill suite).

**Consultation vs Prescription:** Present options with recommendations. Developer decides.

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- Project-specific research conducted during consultation
