---
name: setup-skills
description: >
  Create skill builder and initial skills for the project.
  Reads decisions from task.md and CLAUDE.md, creates project-specific skill builder, generates initial skills.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup skills", "create skill builder", "make skills".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup Skills

**Goal:** Create project-specific skill builder and initial skills based on project conventions.

**Intent:** Skills should follow project conventions discovered during setup-interview and documented in CLAUDE.md.

**Scope:** Create skill builder skill, interview about skill priorities, create initial skills, validate all deliverables.

**Workflow Style:** Hybrid. Imperative bookends (read context, validate), declarative middle (skill creation adapts to needs).

---

## Key Results - KR

1. Skill builder skill created following project conventions
2. Initial skills created based on developer priorities
3. All setup deliverables validated with evidence

## Requirements and Constraints - REQ

1. Read task.md and CLAUDE.md first — understand context before creating
2. Follow the Interaction Mode from CLAUDE.md
3. Track progress per `tracking_and_recovery.md`
4. Interview about skill priorities before creating skills
5. Validate deliverables with positive AND negative evidence

## Preconditions

**Required:**
- task.md from setup-interview
- CLAUDE.md with Interaction Mode and conventions
- Environment set up by setup-environment

**Optional:**
- Existing skills to reference
- Developer's immediate skill needs

## Postconditions

**Success:**
- Skill builder skill created and documented
- Initial skills created per developer priorities
- Validation summary with evidence for each KR
- CLAUDE.md updated with available skills

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify required files exist:
   - task.md at specified path
   - CLAUDE.md with Interaction Mode section
   **If task.md does not exist, STOP and output:** "Cannot proceed. Run `/setup-interview` first to create task.md."
   **If CLAUDE.md does not exist or lacks Interaction Mode, STOP and output:** "Cannot proceed. Run `/setup-environment` first to create CLAUDE.md with Interaction Mode."
1. Read context → Read task.md and CLAUDE.md to understand conventions and Interaction Mode
2. Create preliminary checklist → KR1-3. Use TodoWrite with formula: `skills - KR# - <task>`
3. Create skill builder → KR1. Execute "Create Skill Builder Skill" task
4. Interview about priorities → KR2. Ask developer which skills they need first
5. Create initial skills → KR2. Execute "Create Skills" task
6. Validate deliverables → KR3. Execute "Validate Deliverables" task
7. Update CLAUDE.md → Add available skills section
8. Hand off → Inform user setup is complete, explain how to use skills

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.

### Create Skill Builder Skill (→ KR1)

**Goal:** Create a project-specific skill builder that generates skills following project conventions

**When:** Context read, conventions understood

**Mode:** [implementing](../modes/implementing.md)

**Instructions:**
1. Read [skill-builder-guideline.md](../guidelines/skill-builder-guideline.md)
2. Create skill builder customized to project:
   - Uses project's naming conventions
   - References project's modes and protocols
   - Follows project's workflow style preferences
   - Matches Interaction Mode

**Inputs:**
- task.md (required)
- CLAUDE.md with conventions (required)
- skill-builder-guideline.md (required)

**Outputs:**
- Skill builder skill at `.claude/skills/`
- Documentation of how to use the skill builder

---

### Create Skills (→ KR2)

**Goal:** Create initial skills to get the developer started

**When:** Skill builder created, priorities gathered

**Mode:** Use the skill builder skill created in "Create Skill Builder Skill" task

**Instructions:**
1. Interview developer about which skills they need first
2. Use the skill builder to create each skill
3. Place skills in `.claude/skills/`

**Inputs:**
- Skill builder skill (required)
- Developer's skill priorities (required)

**Outputs:**
- Initial skills customized to project

---

### Validate Deliverables (→ KR3)

**Goal:** Verify all setup KRs met across all three workflows

**When:** All skills created

**Mode:** [evaluating](../modes/evaluating.md)

**Instructions:**
1. For each KR from setup-interview, setup-environment, and setup-skills:
   - Output positive evidence (what confirms it's met)
   - Output negative evidence (what suggests it's not met)
   - Decide if it passes
2. Summarize overall setup success

**Inputs:**
- task.md (required)
- CLAUDE.md (required)
- All created skills (required)

**Outputs:**
- Validation summary with evidence for each KR
- Handoff documentation with usage instructions

**KRs to Validate:**

*From setup-interview:*
1. Interaction Mode established and documented
2. Project state and gaps documented
3. AI-readiness criteria agreed
4. Implementation plan approved

*From setup-environment:*
1. Project folders created per plan
2. CLAUDE.md files created with Interaction Mode
3. Worktree environment configured (if applicable)

*From setup-skills:*
1. Skill builder created
2. Initial skills created
3. Deliverables validated

---

## Additional Notes and Terms

**Skill Placement:** Skills go in `.claude/skills/` or project-specific location from CLAUDE.md.

**Skill Builder Must Emphasize:**
- Maintenance — keep docs up to date
- Tracking — use TodoWrite or trace files
- Validation — evidence-based confirmation
- Understanding use case — interview before creating

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [skill-builder-guideline.md](../guidelines/skill-builder-guideline.md)
- [workflow-creation-guideline.md](../guidelines/workflow-creation-guideline.md)
