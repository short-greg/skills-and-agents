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
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch
---

# Setup Env

**Goal:** Configure AI-ready development environment per decisions from setup-init.

**Intent:** Preferences were established during setup-init. This workflow implements those decisions while respecting Interaction Mode for HOW to execute.

**Scope:** Create project structure (folders, CLAUDE.md with full AI profile, worktrees). No coding conventions (requires tech stack decisions). No skill creation — that's setup-skill-builder.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach per Interaction Mode.

---

## Key Results - KR

1. Project folders created per plan (docs, tasks, etc.)
2. CLAUDE.md created with full AI profile (Interaction Mode, AI Personality, Communication Style)
3. Worktree environment configured (if plan includes it)

## Requirements and Constraints - REQ

1. Read task.md first — do not proceed without it
2. Research current best practices before proposing (use WebSearch)
3. Respect Interaction Mode throughout (see Additional Notes for behavior per mode)
4. Output all proposals as code blocks
5. Do not re-interview WHAT to do — but collaborate on HOW per Interaction Mode
6. Track progress per `tracking_and_recovery.md`

## Preconditions

**Required:**
- task.md from setup-init at `tasks/{TASK_ID}/task.md`
- Implementation plan specifying what to set up

**Optional:**
- Existing project structure to preserve

## Postconditions

**Success:**
- Project folders created per plan
- CLAUDE.md files written with full AI profile
- Worktree configuration complete (if specified)
- setup-skill-builder installed to `.claude/skills/`

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify task.md exists. **If not, STOP:** "Run `/setup-init` first to create task.md."
1. Read task.md → Understand decisions and Interaction Mode
2. Investigate best practices → KR1, KR2. Execute "Investigate Best Practices" task
3. Orient to findings → KR1, KR2, KR3. Execute "Orient to Findings" task
4. Design environment → KR1, KR2, KR3. Execute "Design Environment" task. Output proposal as code block
5. Implement environment → KR1, KR2, KR3. Execute "Implement Environment" task
6. Evaluate results → Execute "Evaluate Results" task
7. Install setup-skill-builder → If successful, copy `workflows/setup/setup-skill-builder.md` to `.claude/skills/setup-skill-builder.md`

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.
3. setup-init must have been executed

### Investigate Best Practices (→ KR1, KR2)

**Goal:** Research current AI-readiness environment patterns

**When:** After reading task.md

**Mode:** [investigating](../../modes/investigating.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Focus on: folder structures, CLAUDE.md best practices, worktree patterns

**Inputs:**
- task.md context (required)

**Outputs:**
- Research findings on current best practices

---

### Orient to Findings (→ KR1, KR2, KR3)

**Goal:** Understand research in context of this project's needs

**When:** After investigation

**Mode:** [orienting](../../modes/orienting.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Compare research findings against task.md requirements
- Note gaps or conflicts

**Inputs:**
- Research findings (required)
- task.md (required)

**Outputs:**
- Contextualized understanding of applicable patterns

---

### Design Environment (→ KR1, KR2, KR3)

**Goal:** Create environment proposal per Interaction Mode

**When:** After orientation

**Mode:** [designing](../../modes/designing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Output proposal as code block
- Collaborate per Interaction Mode (see Additional Notes)

**Inputs:**
- Oriented findings (required)
- task.md decisions (required)

**Outputs:**
- Environment proposal (folders, CLAUDE.md structure, worktrees if applicable)

---

### Implement Environment (→ KR1, KR2, KR3)

**Goal:** Create folders, CLAUDE.md, and worktrees per approved design

**When:** After design approved

**Mode:** [implementing](../../modes/implementing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- CLAUDE.md must include: Interaction Mode, AI Personality, Communication Style
- Follow [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md) if worktrees in plan

**Inputs:**
- Approved design (required)
- task.md with full AI profile (required)

**Outputs:**
- Created folders
- CLAUDE.md with full AI profile
- Worktree configuration (if applicable)

---

### Evaluate Results (→ All KRs)

**Goal:** Verify environment created correctly; hand off if successful

**When:** After implementation

**Mode:** [evaluating](../../modes/evaluating.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Verify CLAUDE.md contains full AI profile
- If successful, output: "Environment ready. Run `/setup-skill-builder` to continue."

**Inputs:**
- Implementation outputs (required)

**Outputs:**
- Verification results
- Hand off to setup-skill-builder (if successful)

---

## Additional Notes and Terms

**Interaction Mode Behavior:**
- **Lead:** Research, propose, execute. Minimal discussion.
- **Senior:** Research, propose, get confirmation before executing.
- **Peer:** Research, present multiple options, debate alternatives, iterate on design together.
- **Junior:** Research, explain each option, get explicit guidance on which to pursue.

**CLAUDE.md Full AI Profile:**
Must include all of these sections:
```markdown
## Interaction Mode

This project uses **[Mode]** collaboration style:
- [Description of what this means for AI behavior]

## AI Personality

- Creativity vs Pragmatism: [Choice]
- Thoroughness vs Speed: [Choice]
- Proactive vs Responsive: [Choice]
- Collaborative vs Direct: [Choice]
- Cautious vs Confident: [Choice]

## Communication Style

- Verbosity: [Concise/Balanced/Detailed]
- Technical Level: [Assume expertise/Explain/Teach]
- Formality: [Professional/Semi-formal/Casual]
```

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
