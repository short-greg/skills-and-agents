---
name: setup-ai-env
description: >
  Configure development environment for AI-readiness based on setup-init decisions.
  Creates folders, CLAUDE.md (with full profile), worktrees. Respects Interaction Mode throughout.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup ai environment", "setup ai env", "create CLAUDE.md", "set up ai project".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion
---

# Setup AI Env

**Goal:** Configure AI-ready development environment per decisions from setup-init.

**Intent:** Preferences were established during setup-init. This workflow implements those decisions while respecting Interaction Mode for HOW to execute.

**Role:** AI Environment Consultant

Think like a consultant who has been hired to make this project AI-ready:
- First understand what's already here (languages, tools, existing config)
- Reason out loud about what you need to know to give good recommendations
- Share your findings and what they imply before asking questions
- Recommend with rationale, not just present options
- Point out risks and considerations the user may not have thought of

**Scope:** Make the environment AI-ready — nothing more. AI-ready means:
- Project context (type, phase) in CLAUDE.md
- Task infrastructure documented (location, template)
- Coding conventions documented in CLAUDE.md (NOT enforced with files)
- Root CLAUDE.md with AI profile
- CLAUDE.local.md template (optional)
- `.claude/` directory structure

**Out of scope:** Creating source code structure, test configs, build files, dependencies, skill creation (setup-skill-builder).

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
7. STRICTLY in scope: .claude/ directory, CLAUDE.md, task location. OUT of scope: source code, tests, build configs

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
7. Install setup-skill-builder → Copy `workflows/setup/setup-skill-builder.md` to `.claude/skills/setup-skill-builder/SKILL.md`

---

## Actions

### A1: Design Environment (→ KR1, KR2, KR3, KR4)

Intent: Understand project context and propose AI environment design
KR: Environment proposal approved by developer
Preconditions:
- Required: task.md with Interaction Mode, AI Personality
Postconditions:
- Success: Output proposal, output explicit approval
- Failure: Output proposal, output feedback for iteration
Exit Conditions:
- Developer rejects 3 times → stop, escalate

Objectives (OBJ):
1. Understand what's already here (detect languages, tools, existing config)
2. Reason out loud about what you need to know to make good recommendations
3. Share findings and reasoning before asking questions
4. Propose with rationale; point out tradeoffs and risks
5. Get explicit approval before A2

Constraints (CONST):
1. Reason out loud about what you need to know before asking
2. Use AskUserQuestion with recommendations, not bare questions
3. Adopt the Interaction Mode and personality from task.md
4. Read `pragmatics.md` and apply Recommended Option — to structure proposals

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

2. Create ONLY AI-related folders per approved design:
   - `.claude/` directory
   - `.claude/feedback/` with README.md and template.md
   - `.claude/skills/` (empty, for future skills)
   - `{TASKS_LOCATION}/` if doesn't exist (from A1)
   - DO NOT create: src/, tests/, config files, language-specific files

3. Write CLAUDE.md with sections per `setup-env-templates.md`:
   - Project Context (type, phase from task.md)
   - Task Infrastructure (location, template structure from A1)
   - Coding Conventions (languages, style guides, frameworks from A1)
   - Interaction Mode + AI Personality (from task.md)
   - Maintenance section

4. Write CLAUDE.local.md template (optional, with examples)

5. Configure worktrees if requested (per worktree-setup-guideline.md)

6. Create `.claude/settings.json` if tooling requested

7. Validate files created:
   - List all files created
   - Verify ONLY .claude/ and approved locations
   - Output summary with what was NOT created

8. Output implementation summary

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

**Tooling Discovery:**

Before recommending tooling, discover what's available:
1. Check existing: `~/.claude/settings.json`, `.claude/settings.json`
2. Check hooks: `~/.claude/hooks/`, `.claude/hooks/`
3. Research: Use WebSearch for MCPs relevant to project type
4. Reason out loud: "For a [project type] project, these MCPs would help because..."

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
| 4 | Scope creep | Creating non-AI files | Validate all files are .claude/* or approved task location; list what was NOT created |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [setup-env-templates.md](setup-env-templates.md)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
