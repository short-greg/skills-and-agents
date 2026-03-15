---
name: setup-env
description: >
  Configure development environment for AI-readiness based on setup-init decisions.
  Creates folders, CLAUDE.md (with full profile), worktrees. Respects Interaction Mode throughout.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup environment", "setup env", "create CLAUDE.md", "set up project structure".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion
---

# Setup Env

**Goal:** Configure AI-ready development environment per decisions from setup-init.

**Intent:** Preferences were established during setup-init. This workflow implements those decisions while respecting Interaction Mode for HOW to execute.

**Scope:** Make the environment AI-ready — nothing more. AI-ready means:
- Project context (type, phase) in CLAUDE.md — drives all AI behavior
- Root CLAUDE.md with AI profile that skills inherit
- CLAUDE.local.md for personal config (gitignored)
- Docs and tasks locations established

**Out of scope:** Coding conventions (requires tech stack), skill creation (setup-skill-builder), legacy cleanup.

**Workflow Style:** Goal-Oriented (Declarative).

---

## Key Results - KR

1. Project folders created per plan (docs, tasks, etc.)
2. CLAUDE.md created with project context + full AI profile
3. Worktree environment configured (if requested)
4. Continuous improvement infrastructure established

## Requirements and Constraints - REQ

1. Read task.md first — do not proceed without it
2. Research current best practices before proposing (use WebSearch)
3. Respect Interaction Mode throughout
4. Track progress per `tracking_and_recovery.md`
5. Get explicit approval before implementing
6. Do NOT assume preferences — always ask via AskUserQuestion

## Preconditions

**Required:** task.md from setup-init at `tasks/{TASK_ID}/task.md`

**Optional:** Existing project structure to preserve

## Postconditions

**Success:** CLAUDE.md written, folders created, setup-skill-builder installed

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify task.md exists. **If not, STOP:** "Run `/setup-init` first."
1. Create preliminary checklist → KR1-4. Use TodoWrite: `setup-env - KR# - <action>`
2. Read task.md → Understand decisions and Interaction Mode
3. Design environment → KR1-4. Execute A1. Output proposal for approval
4. Implement environment → KR1-3. Execute A2
5. Set up continuous improvement → KR4. Execute A3
6. Evaluate results → Execute A4
7. Install setup-skill-builder → Copy to `.claude/skills/`

---

## Actions

### A1: Design Environment (→ KR1, KR2, KR3, KR4)

Intent: Gather decisions and create environment proposal
KR: Environment proposal approved by developer
Preconditions:
- Required: task.md with Interaction Mode, AI Personality
Postconditions:
- Success: Output proposal, output explicit approval
- Failure: Output proposal, output feedback for iteration
Exit Conditions:
- Developer rejects 3 times → stop, escalate

Objectives (OBJ):
1. Echo back task.md understanding, get confirmation
2. Gather required decisions via AskUserQuestion (see Required Decisions below)
3. Generate proposal covering all sections
4. Get explicit approval before A2

Constraints (CONST):
1. Read `thinking.md` and apply Alternative Generation — to explore options
2. Read `pragmatics.md` and apply Option Presentation — to structure proposal
3. Do NOT assume preferences — always ask, make recommendations with rationale

Required Decisions (ask via AskUserQuestion):
- **Project type:** Data science / Web API / Application / Library / CLI / Other
- **Project phase:** Prototyping / Production / Learning / Maintenance
- **Docs location:** recommend based on project type
- **Tasks location:** recommend `tasks/`
- **Task naming:** recommend `YYYYMMDD-<task-name>`
- **Worktrees:** recommend based on project, ALWAYS ask
- **Tooling:** MCP servers, hooks

Validation:
1. Add OBJ1-4 to TodoWrite checklist
2. Output validation with evidence

---

### A2: Implement Environment (→ KR1, KR2, KR3)

Intent: Create folders, CLAUDE.md, worktrees per approved design
KR: Environment artifacts created and verified
Preconditions:
- Required: Approved design from A1
Postconditions:
- Success: Output folders created, CLAUDE.md written
- Failure: Output errors documented
Exit Conditions:
- Design not approved → stop, complete A1 first

Instructions:
1. Read `discipline.md` and apply Sequential Processing
2. Create folders per design
3. Write CLAUDE.md per `setup-env-templates.md` — translate task.md into behavioral instructions
4. Configure worktrees if requested (per worktree-setup-guideline.md)
5. Create `.claude/settings.json` if tooling requested
6. Output implementation summary

---

### A3: Set Up Continuous Improvement (→ KR4)

Intent: Establish feedback tracking infrastructure
KR: CLAUDE.md maintenance system and feedback tracking in place
Preconditions:
- Required: CLAUDE.md created
Postconditions:
- Success: Output feedback folder created, maintenance section added
- Failure: Output what could not be created
Exit Conditions:
- CLAUDE.md not found → stop, complete A2 first

Instructions:
1. Create `.claude/feedback/` with README.md and template.md
2. Add Maintenance section to CLAUDE.md
3. Output summary

---

### A4: Evaluate Results (→ All KRs)

Intent: Verify environment created correctly
KR: All KRs verified with evidence
Preconditions:
- Required: Implementation outputs
Postconditions:
- Success: Output verification results, hand off message
- Failure: Output failures with remediation
Exit Conditions:
- Critical failure → stop, await guidance

Instructions:
1. Read `software_quality.md` and apply Correctness — to assess artifacts
2. Confirm CLAUDE.md contains project context + AI profile
3. Confirm folders exist per plan
4. Output verification with evidence for each KR
5. If successful: "Environment ready. Run `/setup-skill-builder` to continue."

---

## Additional Notes and Terms

**Required Decisions Summary:**
| Category | Decisions |
|----------|-----------|
| Project Context | Type (data science/web API/app/library/CLI), Phase (prototyping/production/learning/maintenance) |
| Structure | Docs location, Tasks location, Task naming scheme, Worktrees |
| Tooling | MCP servers, Hooks |

**Project Phase → AI Behavior:**
| Phase | Behavior |
|-------|----------|
| Prototyping | Speed over maintainability. Minimal structure. Quick experiments. |
| Production | Careful architecture. Comprehensive tests. Conservative changes. |
| Learning | Teaching mode. Explanations. Step-by-step guidance. |
| Maintenance | Preserve patterns. Conservative changes. Thorough testing. |

**CLAUDE.md Strategy:**
| File | Purpose | Gitignored? |
|------|---------|-------------|
| `/CLAUDE.md` | AI profile, project context. Skills inherit this. | No |
| `/CLAUDE.local.md` | Personal settings, local paths. | Yes |
| Directory CLAUDE.md | Context for that directory (optional). | No |

**Skill-Builder Inheritance:** Skills created by setup-skill-builder will read root CLAUDE.md to inherit project context and AI profile. This is why project type/phase must be captured here.

**Templates:** See `setup-env-templates.md` for proposal template, CLAUDE.md example, and feedback template.

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Overwriting existing files | CLAUDE.md exists | Confirm before overwriting |
| 2 | Assumptions made | Skipping AskUserQuestion | Always ask, never assume |
| 3 | task.md drift | Implementation diverges | Reference task.md at each step |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [setup-env-templates.md](setup-env-templates.md)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
