---
name: ai-readiness-consultant
description: >
  Consultative workflow to make a project AI-ready. Works with developer to understand their
  project, recommend approaches, and deliver a project-specific skill builder.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "set up AI environment", "make project AI-ready", "AI readiness consultation".
argument-hint: "[project path or context]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# AI Readiness Consultant

**Goal:** Partner with developer to make their project AI-ready and deliver a project-specific skill builder.

**Intent:** Projects differ in structure, conventions, and needs. A prescriptive setup fails to fit. This workflow acts consultatively—understanding the project, presenting options, and creating customized deliverables.

**Scope:** Full consultation from assessment through delivery. Consultant recommends but developer decides.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach to project needs.

---

## Key Results - KR

1. Project's current state and AI-readiness gaps documented
2. Project-specific definition of "AI-ready" established with developer agreement
3. Implementation plan created and approved
4. Project-specific skill builder delivered encoding project conventions
5. The Repo has been made to be AI-ready

## Requirements and Constraints - REQ

[COMMENT: Keep phrased as action]
1. Track progress per `tracking_and_recovery.md` — trace file with action summaries and decisions use both a todo list and a recovery file
2. Ensure recoverable from interruption — check for existing trace on startup
3. Consultative not prescriptive — recommend with rationale, developer decides
4. Web research during consultation — current best practices for skill builders and setup

---

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

1. Create preliminary checklist → KR1-5, uses TodoWrite — formula: `ai-readiness - KR# - <task> - <details>`
2. Position consultation approach → KR2, uses `positioning.md` — output how you will consult
3. Orient to project → KR1, uses `orienting.md` — review all project documentation
4. Interview developer → KR1, KR2, uses `interviewing.md` — gather needs and preferences
5. Compose project-specific checklist → KR1-5 — update checklist with tasks to satisfy all KRs for this project
6. Execute checklist tasks → KR3, KR4, KR5 — complete all tasks, updating checklist as you go
7. Validate all KRs → KR1-5, uses `evaluating.md` — output evidence for and against each KR passing

---

## Tasks

**IMPORTANT:** Each task specifies modes to use. When executing a task you MUST read those modes if you haven't already. Select tasks based on context. Each task shows which KR it serves.

### Recover from Interruption (→ KR1, KR2, KR3, KR4)

**Goal:** Resume consultation with full context

**When:** Trace file exists from previous session

**Instructions:** Read trace, determine last completed action, present summary to developer, confirm how to proceed.

**Inputs:**
- Trace file (required)

**Default Output:** Recovery summary and confirmed resume point

---

### Assess Current State (→ KR1)

**Goal:** Understand project and identify AI-readiness gaps

**When:** Starting consultation or need to understand project

**Mode:** [orienting](../modes/orienting.md), [investigating](../modes/investigating.md)

**Instructions:** Orient to project structure, conventions, documentation. Investigate code for implicit patterns. Do web research on AI-readiness best practices for similar projects. Document findings in trace.

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Default Output:** Assessment document with current state and gaps

---

### Gather Requirements (→ KR1, KR2)

**Goal:** Understand developer's needs and preferences

**When:** Need to understand what developer wants

**Mode:** [interviewing](../modes/interviewing.md)

**Instructions:** Gather project context, team context, pain points, customization preferences. Use funnel sequencing. Recommend customization level based on findings.

**Inputs:**
- Assessment findings (optional)
- Prior conversation context (optional)

**Default Output:** Requirements summary with developer confirmation

---

### Define AI-Readiness Criteria (→ KR2)

**Goal:** Establish what "AI-ready" means for this project

**When:** Assessment complete, ready to define scope

**Mode:** [defining](../modes/defining.md), [positioning](../modes/positioning.md)

**Instructions:** Present AI-readiness dimensions as options. Recommend priorities based on assessment. Let developer choose focus areas. Document agreed criteria.

AI-Readiness dimensions to consider:
- Understanding (documentation, CLAUDE.md)
- Navigation (structure, conventions)
- Behavior knowledge (tests, specifications)
- Task execution (instructions, examples)
- Skills (workflows, modes)
- Constraints (requirements, boundaries)
- Growth capability (skill builder)
- Maintainability (updates, versioning)

**Inputs:**
- Assessment document (required)
- Developer preferences (optional)

**Default Output:** Definition document with agreed criteria

---

### Plan Implementation (→ KR3)

**Goal:** Create roadmap for achieving AI-readiness

**When:** Definition complete, ready to plan

**Mode:** [planning](../modes/planning.md)

**Instructions:** Based on definition, plan deliverables. Do web research on current skill builder patterns. Present plan with options. Let developer approve or modify.

**Inputs:**
- Definition document (required)
- Constraints (optional)

**Default Output:** Approved implementation plan

---

### Design Skill Builder (→ KR4)

**Goal:** Design project-specific skill builder

**When:** Plan approved, ready to design

**Mode:** [designing](../modes/designing.md)

**Instructions:** Design skill builder encoding project conventions. Do web research on current patterns if needed. Present design for feedback.

Skill builder should encode:
- CLAUDE.md review requirement
- Repo structure patterns
- Document locations
- Task directories
- Naming conventions
- Project-specific constraints

**Inputs:**
- Implementation plan (required)
- Project conventions (required)

**Default Output:** Skill builder design document

---

### Implement Deliverables (→ KR4)

**Goal:** Create skill builder and supporting artifacts

**When:** Design approved, ready to implement

**Mode:** [implementing](../modes/implementing.md)

**Instructions:** Create skill builder per design. Create supporting documentation. Update CLAUDE.md if needed. Present drafts, iterate on feedback.

**Inputs:**
- Design document (required)
- Project structure (required)

**Default Output:** Skill builder and supporting artifacts

---

### Validate Deliverables (→ KR1, KR2, KR3, KR4)

**Goal:** Verify all KRs met and hand off

**When:** Implementation complete

**Mode:** [evaluating](../modes/evaluating.md)

**Instructions:** Verify assessment reflects project state, definition criteria met, skill builder encodes conventions correctly. Present validation summary. Provide handoff with usage instructions and next steps.

**Inputs:**
- All deliverables (required)
- KR criteria (required)

**Default Output:** Validation summary and handoff documentation

---

### Set Up Project Structure (→ KR5)

**Goal:** Create folder structure for AI-ready project

**When:** Plan includes folder structure recommendations

**Mode:** [implementing](../modes/implementing.md)

**Instructions:** Based on plan, create recommended folders (tasks/, docs/, etc.). Adapt to existing project structure. Present options if unclear where folders should go.

**Inputs:**
- Implementation plan (required)
- Existing project structure (required)

**Default Output:** Created folders with explanation of purpose

---

### Set Up Worktree Environment (→ KR5)

**Goal:** Configure project for git worktree workflows

**When:** Developer wants parallel task execution capability

**Mode:** [implementing](../modes/implementing.md)

**Instructions:** Consult current worktree setup best practices. Present options for worktree configuration. Create setup based on developer choice. Reference external worktree guidelines if available.

**Inputs:**
- Project git structure (required)
- Developer preference on worktree approach (optional)

**Default Output:** Worktree configuration with usage instructions

---

### Set Up CLAUDE.md Documentation (→ KR5)

**Goal:** Create or update CLAUDE.md files throughout repo

**When:** Documentation gaps identified in assessment

**Mode:** [orienting](../modes/orienting.md), [implementing](../modes/implementing.md)

**Instructions:** Review repo structure to identify packages/modules needing CLAUDE.md. Create root CLAUDE.md with project conventions. Create package-level CLAUDE.md files as needed. Document patterns discovered during orientation.

**Inputs:**
- Repo structure (required)
- Discovered conventions (required)

**Default Output:** CLAUDE.md files at appropriate levels

---

## Additional Notes and Terms

**Skill Builder:** Generated skill that creates other skills. Encodes project conventions so future skills follow patterns automatically.

**Customization Levels:**
- Minimal — Basic CLAUDE.md, standard structure
- Moderate — Project-specific patterns documented
- Comprehensive — Full skill suite, detailed conventions

**Consultation vs Prescription:** Present options with recommendations. Developer makes final choices. Unlisted options valid if they fit.

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- Project-specific research conducted during consultation
