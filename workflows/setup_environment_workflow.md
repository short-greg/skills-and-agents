---
name: setup-environment
description: >
  Execute environment setup based on decisions from setup-interview.
  Creates folders, CLAUDE.md files, worktrees. No heavy interviewing — decisions already made.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup environment", "create CLAUDE.md", "set up project structure".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite
---

# Setup Environment

**Goal:** Execute environment setup per the approved plan from setup-interview.

**Intent:** Decisions were made during setup-interview. This workflow implements those decisions without re-interviewing.

**Scope:** Create project structure (folders, CLAUDE.md, worktrees) based on task.md. No skill creation — that's setup-skills.

**Workflow Style:** Process-Oriented (Imperative). Execute the plan as specified.

---

## Key Results - KR

1. Project folders created per plan (docs, tasks, etc.)
2. CLAUDE.md files created with Interaction Mode and project conventions
3. Worktree environment configured (if plan includes it)

## Requirements and Constraints - REQ

1. Read task.md first — do not proceed without it
2. Follow the Interaction Mode specified in task.md
3. Track progress per `tracking_and_recovery.md`
4. Do not re-interview — execute what was decided
5. Confirm actions per Interaction Mode (Junior = confirm all, Lead = confirm none)

## Preconditions

**Required:**
- task.md from setup-interview at `tasks/{TASK_ID}/task.md`
- Implementation plan specifying what to set up

**Optional:**
- Existing project structure to preserve

## Postconditions

**Success:**
- Project folders created per plan
- CLAUDE.md files written with Interaction Mode documented
- Worktree configuration complete (if specified)
- Ready for setup-skills workflow

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify task.md exists at specified path. **If task.md does not exist, STOP and output:** "Cannot proceed. Run `/setup-interview` first to create task.md with project decisions."
1. Read task.md → Understand all decisions from setup-interview
2. Read Interaction Mode → Determine confirmation level needed
3. Create project folders → KR1. Execute "Set Up Project Folders" task
4. Create CLAUDE.md files → KR2. Execute "Create CLAUDE.md Files" task
5. Set up worktrees → KR3. Execute "Set Up Worktree Environment" task (if plan includes it)
6. Hand off → Inform user to invoke `/setup-skills` next

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.
3. setup-interview must have been executed

### Set Up Project Folders (→ KR1)

**Goal:** Create project folder structure per plan

**When:** task.md has been read, plan specifies folder creation. 

**Mode:** [implementing](../modes/implementing.md)

**Instructions:**
1. Read plan from task.md
2. Create folders as specified (docs, tasks, etc.)
3. Confirm per Interaction Mode before creating

**Inputs:**
- task.md (required)
- Implementation plan (required)

**Outputs:**
- Created folders with purpose explanation

---

### Create CLAUDE.md Files (→ KR2)

**Goal:** Create CLAUDE.md files documenting project conventions and Interaction Mode

**When:** Folders created

**Mode:** [implementing](../modes/implementing.md)

**Instructions:**
1. Create root CLAUDE.md with:
   - **Interaction Mode** — how AI should collaborate (Lead/Senior/Peer/Junior)
   - Project overview
   - Codebase navigation guide
   - Key conventions
   - Available skills (will be updated by setup-skills)
2. Create package-level CLAUDE.md if monorepo (per plan)
3. Emphasize: how AI should navigate codebase, important patterns/anti-patterns

**Inputs:**
- task.md with Interaction Mode (required)
- Project structure (required)
- Assessment from setup-interview (required)

**Outputs:**
- Root CLAUDE.md with Interaction Mode documented
- Package-level CLAUDE.md files (if monorepo)

---

### Set Up Worktree Environment (→ KR3)

**Goal:** Configure git worktrees for parallel development workflows

**When:** Plan includes worktree setup

**Mode:** [implementing](../modes/implementing.md)

**Instructions:**
1. Read worktree requirements from task.md
2. Follow [worktree-setup-guideline.md](../guidelines/worktree-setup-guideline.md)
3. Create configuration, shell functions, .worktreesync

**Inputs:**
- task.md with worktree decisions (required)
- Project structure (required)

**Outputs:**
- Worktree configuration
- Shell functions
- .worktreesync file
- Orchestration workflows (if needed)

---

## Additional Notes and Terms

**Interaction Mode Confirmation Levels:**
- **Lead:** Execute without confirmation
- **Senior:** Confirm major decisions only
- **Peer:** Discuss approach, confirm execution
- **Junior:** Confirm every action before executing

**CLAUDE.md Interaction Mode Section:**
```markdown
## Interaction Mode

This project uses **[Mode]** collaboration style:
- [Description of what this means for AI behavior]
```

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [worktree-setup-guideline.md](../guidelines/worktree-setup-guideline.md)
