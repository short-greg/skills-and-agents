# Git Worktree Guidelines v0.1

A lightweight guide for using Git worktrees to run multiple AI coding assistant sessions in parallel.

## Tool Compatibility

This guide was written for Claude Code but applies to any AI coding assistant (Codex, Cursor, GitHub Copilot, etc.).

**For AI Coders:** Adapt these instructions to your tool's conventions:
- Replace `.claude/skills/` with your tool's skills directory (e.g., `.codex/skills/`, `.cursor/prompts/`)
- Replace `claude` command with your tool's CLI command
- Adapt skill file format to your tool's requirements
- Update `.worktreesync` to include your tool's config directory

The core concepts (worktrees, orchestration, specs, handover) remain the same across all tools.

Ask the developer where the specs/plans folder is.

### Pre-flight Check for AI Coders

Before using these skills, validate the project structure:

**Check 1: Directory Structure**
```bash
# Expected structure:
project-base/
├── project/              # Main repo (current location)
│   ├── .worktreesync     # Should exist or be created
│   └── <specs/plans folder>/         # Should exist or be created
└── Worktrees/            # Should exist at ../Worktrees

# Verify:
ls ../Worktrees 2>/dev/null || echo "WARNING: ../Worktrees does not exist"
```

**If structure doesn't match:**
- **Warning:** "This project doesn't follow the expected `-base` directory structure. The worktree scripts expect `../Worktrees` to exist. Create it with `mkdir ../Worktrees` or ask the developer to reorganize."

**Check 2: Tool Config Directory**
```bash
# Claude Code expects:
ls .claude/skills/ 2>/dev/null || echo "Skills directory not found"

# Codex expects:
ls .codex/skills/ 2>/dev/null || echo "Skills directory not found"
```

**If missing:**
- **Warning:** "Skills directory not found. Create it with `mkdir -p .{tool}/skills/{orchestrate-parallel-tasks,worktree-help}` or ask the developer if this project uses a custom skills location."

**Check 3: Shell Functions**

The worktree workflow requires helper shell functions (`git-worktree-add`, `git-worktree-remove`). Check if they are available:

```bash
type git-worktree-add 2>/dev/null || echo "Shell functions not installed"
```

**If missing**, guide the developer through installation:
1. Determine the developer's shell (`echo $SHELL` or ask them — common options: zsh, bash, PowerShell)
2. Show the appropriate shell functions from the "Add Shell Functions" section below (zsh/bash) or "Windows Setup" section (PowerShell)
3. Instruct them to add the functions to their shell config file:
   - **zsh:** `~/.zshrc`
   - **bash:** `~/.bashrc`
   - **PowerShell:** `$PROFILE` (find with `echo $PROFILE`)
4. Instruct them to reload their shell config:
   - **zsh:** `source ~/.zshrc`
   - **bash:** `source ~/.bashrc`
   - **PowerShell:** `. $PROFILE`
5. Confirm installation: `type git-worktree-add`

## Overview

Git worktrees allow you to have multiple working directories from the same repository, each on a different branch. This enables running parallel Claude Code sessions on separate tasks without conflicts.

**Key Limitation**: Git only tracks indexed files. Worktrees do NOT include `.gitignore`d files like `.env`, `node_modules/`, or build caches.

## Setup

### 1. Organize Your Repository

Use a `-base` parent folder structure:

```bash
# If starting fresh
mkdir my-project-base
cd my-project-base
git clone <repo-url> my-project
mkdir Worktrees

# If you have an existing repo, move it into a -base folder
mkdir my-project-base
mv my-project my-project-base/
cd my-project-base
mkdir Worktrees
```

This ensures `../Worktrees` always resolves correctly from your main repo.

### 2. Add Shell Functions

Add the following to your `~/.zshrc` (or `~/.bashrc`):

```bash
# Git Worktree Add - Creates worktree with environment sync
git-worktree-add() {
    local WT_PARENT="../Worktrees"

    if [ ! -d "$WT_PARENT" ]; then
        echo "ERROR: The folder '$WT_PARENT' does not exist."
        echo "Please create it first: mkdir $WT_PARENT"
        return 1
    fi

    if [ "$#" -lt 2 ]; then
        echo "Usage: git-worktree-add <name> <branch>"
        echo ""
        echo "Creates a new Git worktree with environment file sync."
        echo ""
        echo "Examples:"
        echo "  git-worktree-add my-feature feat/my-feature"
        echo "  git-worktree-add bugfix fix/login-issue"
        echo ""
        echo "After creation:"
        echo "  cd ../Worktrees/<name>"
        echo "  claude"
        echo ""
        echo "Config: Add files to .worktreesync to sync untracked files"
        return 1
    fi

    local NAME=$1
    local BRANCH=$2
    local TARGET="$WT_PARENT/$NAME"
    local INCLUDE_FILE=".worktreesync"

    # Create the worktree
    git worktree add "$TARGET" -b "$BRANCH"

    # Sync environment files if config exists
    if [ -f "$INCLUDE_FILE" ]; then
        echo "Syncing environment from $INCLUDE_FILE..."
        while IFS= read -r item || [ -n "$item" ]; do
            [[ -z "$item" || "$item" =~ ^# || ! -e "$item" ]] && continue
            cp -r "$item" "$TARGET/$item"
            echo "  + $item"
        done < "$INCLUDE_FILE"
    else
        echo "No $INCLUDE_FILE found; created standard worktree."
    fi

    # Initialize submodules in the new worktree
    if [ -f "$TARGET/.gitmodules" ]; then
        echo "Initializing submodules..."
        git -C "$TARGET" submodule update --init
    fi

    echo "Successfully created worktree at: $TARGET"
}

# Git Worktree Remove - Cleans up worktree and metadata
git-worktree-remove() {
    local WT_PARENT="../Worktrees"

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-remove <name>"
        echo ""
        echo "Removes a Git worktree and cleans up metadata."
        echo ""
        echo "Examples:"
        echo "  git-worktree-remove my-feature"
        echo ""
        echo "Note: Close all terminals in the worktree first!"
        echo "List worktrees: git worktree list"
        return 1
    fi

    local NAME=$1
    local TARGET="$WT_PARENT/$NAME"

    if git worktree remove "$TARGET"; then
        git worktree prune
        echo "Successfully removed $NAME and cleaned up metadata."
    else
        echo "Error: Could not remove $TARGET."
        echo "Ensure no terminals are open in that directory."
        return 1
    fi
}
```

Reload your shell:

```bash
source ~/.zshrc
```

### 3. Create a `.worktreesync` File (Recommended)

Without this file, essential untracked files (like `.env`) won't be copied to new worktrees. In your project root, create a `.worktreesync` file listing files/folders to sync:

```bash
touch .worktreesync
```

Example contents:

```
# Environment files
.env
.env.local

# IDE settings
.vscode/settings.json

# Any other untracked local config needed for development
```

**Note**: Without this file, worktrees are created without any untracked files (the script will show a warning but continue). Add `.worktreesync` to your `.gitignore` if it contains sensitive paths.

## Usage

### Creating a Worktree

```bash
# From your main project directory
git-worktree-add my-feature feat/my-feature

# Start Claude in the new worktree
cd ../Worktrees/my-feature
claude
```

### Removing a Worktree

```bash
# Exit Claude and cd out of the worktree first!
cd /path/to/main-project
git-worktree-remove my-feature
```

**Important**: Git prevents removal if a terminal is open in that directory.

### Listing Worktrees

```bash
git worktree list
```

## Workflow Example

```bash
# 1. Create worktrees for parallel tasks
git-worktree-add api-refactor feat/api-refactor
git-worktree-add ui-update feat/ui-update

# 2. Run Claude in each (separate terminals)
cd ../Worktrees/api-refactor && claude
cd ../Worktrees/ui-update && claude

# 3. When done, merge branches and clean up
cd /path/to/main-project
git merge feat/api-refactor
git merge feat/ui-update
git-worktree-remove api-refactor
git-worktree-remove ui-update
```

## Tips

### Symlink `node_modules` (Speed Hack)

If tasks don't add new dependencies, save disk space by symlinking:

```bash
cd ../Worktrees/my-feature
ln -s ../../main-project/node_modules ./node_modules
```

**Caution**: Only works if you aren't installing new packages in the worktree.

### Use `pnpm` or `bun`

These package managers use global content-addressable stores. Running `pnpm install` in a new worktree is near-instant because it hard-links from the global cache.

### Recommended Directory Structure

Organize your repo inside a `-base` parent folder:

```
my-project-base/             # Parent folder
├── .venv/                   # Shared virtual environment (optional)
├── my-project/              # Main repo
│   ├── .worktreesync        # Defines files to sync
│   └── ...
└── Worktrees/               # All worktrees live here
    ├── feature-a/
    ├── feature-b/
    └── bugfix-c/
```

This keeps the main repo and worktrees as siblings, making `../Worktrees` consistent and predictable.

### Shared Virtual Environment (Python)

Place a shared venv in the base folder so all worktrees can use it:

```bash
# Create shared venv in base folder
cd my-project-base
python -m venv .venv

# Activate from main repo
source ../.venv/bin/activate

# Activate from a worktree
source ../../.venv/bin/activate
```

**Note on editable installs**: If your project uses `pip install -e ./submodule`, the editable install embeds an absolute path. You'll need to run the install command in each worktree if you want changes in that worktree's submodule to be reflected. This is a manual step - the shell function does not handle it.

### Submodules

Submodules are automatically initialized when creating a worktree (if `.gitmodules` exists). However, any local setup steps (like `pip install -e ./framework`) must be run manually in each worktree.

## Windows Setup

For Windows users using PowerShell, add these functions to your `$PROFILE`:

```powershell
# Git Worktree Add (PowerShell)
function git-worktree-add {
    param(
        [Parameter(Mandatory=$true)][string]$Name,
        [Parameter(Mandatory=$true)][string]$Branch
    )

    $WtParent = "..\Worktrees"

    if (-not (Test-Path $WtParent)) {
        Write-Error "ERROR: The folder '$WtParent' does not exist."
        Write-Host "Please create it first: mkdir $WtParent"
        return
    }

    $Target = Join-Path $WtParent $Name
    $IncludeFile = ".worktreesync"

    git worktree add $Target -b $Branch

    if (Test-Path $IncludeFile) {
        Write-Host "Syncing environment from $IncludeFile..."
        Get-Content $IncludeFile | ForEach-Object {
            $item = $_.Trim()
            if ($item -and -not $item.StartsWith("#") -and (Test-Path $item)) {
                Copy-Item -Path $item -Destination (Join-Path $Target $item) -Recurse -Force
                Write-Host "  + $item"
            }
        }
    } else {
        Write-Host "No $IncludeFile found; created standard worktree."
    }

    # Initialize submodules in the new worktree
    $GitModules = Join-Path $Target ".gitmodules"
    if (Test-Path $GitModules) {
        Write-Host "Initializing submodules..."
        git -C $Target submodule update --init
    }

    Write-Host "Successfully created worktree at: $Target"
}

# Git Worktree Remove (PowerShell)
function git-worktree-remove {
    param(
        [Parameter(Mandatory=$true)][string]$Name
    )

    $Target = Join-Path "..\Worktrees" $Name

    $result = git worktree remove $Target 2>&1
    if ($LASTEXITCODE -eq 0) {
        git worktree prune
        Write-Host "Successfully removed $Name and cleaned up metadata."
    } else {
        Write-Error "Could not remove $Target. Ensure no terminals are open in that directory."
    }
}
```

To find your PowerShell profile location: `echo $PROFILE`

To create/edit it: `notepad $PROFILE`

After saving, reload with: `. $PROFILE`

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Worktrees folder does not exist" | Run `mkdir ../Worktrees` |
| "Could not remove worktree" | Close all terminals in that directory first |
| Missing `.env` in worktree | Add `.env` to `.worktreesync` and recreate |
| Branch already exists | Use existing branch: `git worktree add ../Worktrees/name existing-branch` |

## Skills for Worktree Workflows

**Note:** This section uses Claude Code's `.claude/skills/` convention. If using a different AI tool:
- Codex: Replace `.claude/` with `.codex/`
- Cursor: Replace `.claude/` with `.cursor/` (or your tool's equivalent)
- Other tools: Adapt to your tool's skill/prompt system

The skill logic and workflow remain the same regardless of tooling.

### `/orchestrate-parallel-tasks`
**The main orchestrator** - handles the full lifecycle of parallel work with dependency tracking.

| Command | Description |
|---------|-------------|
| `/orchestrate-parallel-tasks` | Show current state and options |
| `/orchestrate-parallel-tasks plan` | Break PRD into tasks with dependency tracking |
| `/orchestrate-parallel-tasks setup` | Create worktrees (only for tasks with satisfied dependencies) |
| `/orchestrate-parallel-tasks status` | Check progress and show blocked/ready tasks |
| `/orchestrate-parallel-tasks review` | Code review completed branches before merging |
| `/orchestrate-parallel-tasks merge` | Merge approved branches in dependency order, unblock waiting tasks |

**Workflow with dependencies:**
1. **Plan phase**: Break PRD into tasks, identify dependencies, save to `<specs/plans folder>/YYYY-MM-DD-parallel-plan.md`
2. **Setup phase**: Only create worktrees for tasks with no dependencies or whose dependencies are already merged
3. **Status phase**: Show which tasks are blocked waiting for dependencies
4. **Merge phase**: Merge in correct order, notify when tasks become unblocked

**How merging works:** The orchestrator merges worktree branches into **its current branch** in dependency order. Each worktree session works on its own branch and must commit before the orchestrator can merge.

**Key step often missed during setup:** Spec files in `<specs/plans folder>/` are typically gitignored. The skill must:
```bash
# Copy spec to worktree
cp <specs/plans folder>/YYYY-MM-DD-{task}-spec.md ../Worktrees/{task}/<specs/plans folder>/

# Force-add and commit (since <specs/plans folder>/* may be gitignored)
cd ../Worktrees/{task}
git add -f <specs/plans folder>/YYYY-MM-DD-{task}-spec.md
git commit -m "spec: add {task} specification"
```

### `/create-example`
**Worker skill** - runs inside each worktree session to implement the task.

**What it does:**
0. **Dependency check** - Verify all dependencies are merged before starting
1. Research - Read framework docs and existing examples
2. Proposal - Present implementation plan for approval
3. Implementation - Write the code
4. Grievances - Document friction points
5. Integration - Register in app.py, test
6. **Commit all work** - Critical for handover to orchestrator

**Expects:** A spec document at `<specs/plans folder>/YYYY-MM-DD-*-spec.md` with a Dependencies section

**Dependency enforcement:** If the task depends on other branches, the skill checks if they're merged using `git branch --merged`. If dependencies aren't satisfied, it **stops immediately** and instructs the user to complete dependencies first.

**Critical handover step:** Worker sessions MUST commit all work before finishing. The orchestrator merges worktree branches into its own branch, so uncommitted work will not be merged.

### `/worktree-help`
**Reference skill** - shows worktree commands and troubleshooting.

## Identifying Worktree Sessions

When running multiple Claude sessions, it helps to identify which terminal is which.

### Terminal Tab Titles

Set the terminal title before launching your AI coding tool:

```bash
# Example for each worktree (Claude Code)
echo -ne "\033]0;Example 11 - Parallel BT\007" && cd ../Worktrees/example11-parallel-bt && claude

# For other tools, replace 'claude' with your tool's command:
# codex, cursor, copilot-cli, etc.
echo -ne "\033]0;Example 12 - BT Decorators\007" && cd ../Worktrees/example12-bt-decorators && codex
```

### iTerm2 Badge (macOS)

For iTerm2 users, set a badge that overlays the terminal:

```bash
printf "\e]1337;SetBadgeFormat=%s\a" $(echo -n "Example 11" | base64)
```

### Tool Status Line (Claude Code Example)

Many AI coding tools show a status line. For Claude Code, configure it in `.claude/settings.local.json`:

```json
{
  "statusLine": {
    "showBranch": true
  }
}
```

The branch name (which matches the worktree name) will be visible in the status line.

**For other tools:** Check your tool's documentation for status line or branch display configuration.

### Other Options

- **Different terminal color profiles** - Assign each worktree a different theme
- **tmux/screen pane names** - If using a multiplexer, name each pane
- **Conversation title** - Claude shows the conversation title in the UI

## Workflow Summary

### Standard Workflow (Independent Tasks)

```
┌─────────────────────────────────────────────────────────────┐
│  Main Session (Orchestrator)                                │
│                                                             │
│  1. /orchestrate-parallel-tasks setup                       │
│     - Define tasks with user                                │
│     - Create worktrees                                      │
│     - Commit specs into each worktree                       │
│     - Output instructions for starting sessions             │
│                                                             │
│  2. User starts Claude in each worktree (separate terminals)│
│                                                             │
│  3. /orchestrate-parallel-tasks status                      │
│     - Check progress anytime                                │
│                                                             │
│  4. /orchestrate-parallel-tasks review                      │
│     - Code review each branch                               │
│     - Check coding standards, best practices                │
│     - Request changes if needed                             │
│                                                             │
│  5. /orchestrate-parallel-tasks merge                       │
│     - Merge approved branches                               │
│     - Run tests after each merge                            │
│     - Clean up worktrees                                    │
│     - Delete branches                                       │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  Worktree Session (Worker)                                  │
│                                                             │
│  1. Read spec at <specs/plans folder>/YYYY-MM-DD-{task}-spec.md         │
│                                                             │
│  2. /create-example                                         │
│     - Check dependencies (Phase 0)                          │
│     - Research                                              │
│     - Propose                                               │
│     - Implement                                             │
│     - Document grievances                                   │
│                                                             │
│  3. Commit and notify user when done                        │
└─────────────────────────────────────────────────────────────┘
```

### Workflow with Dependencies

```
┌─────────────────────────────────────────────────────────────┐
│  Main Session (Orchestrator)                                │
│                                                             │
│  1. /orchestrate-parallel-tasks plan                        │
│     - Break PRD into tasks                                  │
│     - Identify dependencies                                 │
│     - Create dependency graph                               │
│     - Save plan to <specs/plans folder>/YYYY-MM-DD-parallel-plan.md     │
│                                                             │
│  2. /orchestrate-parallel-tasks setup                       │
│     - Read plan, identify Phase 1 tasks (no dependencies)   │
│     - Create worktrees ONLY for ready tasks                 │
│     - Commit specs                                          │
│                                                             │
│  3. User starts sessions for Phase 1 tasks                  │
│                                                             │
│  4. /orchestrate-parallel-tasks status                      │
│     - Show active tasks                                     │
│     - Show blocked tasks waiting for dependencies           │
│                                                             │
│  5. /orchestrate-parallel-tasks review (Phase 1)            │
│     - Review completed branches                             │
│                                                             │
│  6. /orchestrate-parallel-tasks merge (Phase 1)             │
│     - Merge in dependency order                             │
│     - Notify: "Task X is now unblocked"                     │
│                                                             │
│  7. /orchestrate-parallel-tasks setup (Phase 2)             │
│     - Create worktrees for newly unblocked tasks            │
│                                                             │
│  8. Repeat steps 3-7 for each phase until all complete      │
└─────────────────────────────────────────────────────────────┘

Example dependency flow:
  Phase 1 (parallel): task1, task2
  Phase 2 (after Phase 1 merges): task3 (needs task1), task4 (needs task2)
  Phase 3 (after Phase 2 merges): task5 (needs task3 AND task4)
```

## Setting Up Skills

Skills live in `.claude/skills/{skill-name}/SKILL.md`. To enable parallel worktree workflows, create these three skills:

### Directory Structure

```
.claude/
└── skills/
    ├── orchestrate-parallel-tasks/
    │   └── SKILL.md
    ├── create-example/       # Or your worker skill name
    │   └── SKILL.md
    └── worktree-help/
        └── SKILL.md
```

### Skill 1: `/orchestrate-parallel-tasks`

**File:** `.claude/skills/orchestrate-parallel-tasks/SKILL.md`

**Purpose:** Main orchestrator for parallel work lifecycle with dependency tracking (plan, setup, status, review, merge).

**Key sections to include:**
- Frontmatter with description and argument-hint
- Phase 0: Plan - break PRD into tasks, identify dependencies, create `<specs/plans folder>/YYYY-MM-DD-parallel-plan.md`
- Phase 1: Setup - read plan, only create worktrees for tasks with satisfied dependencies
- Phase 2: Status - show active tasks, blocked tasks, and dependency status
- Phase 3: Review - code review with checklist before merging
- Phase 4: Merge - merge in dependency order, notify when tasks become unblocked

**Critical implementation details:**

Task spec template must include:
```markdown
## Dependencies
**This task depends on:** [list or "None"]
**Blocks these tasks:** [list or "None"]
**Status:** Ready to start / Waiting for dependencies
```

When committing specs to worktrees:
```bash
cd ../Worktrees/{task}
git add -f <specs/plans folder>/*.md  # Force-add gitignored files
git commit -m "spec: add {task} specification"
```

When merging:
- The orchestrator merges worktree branches **into its current branch**
- Worker sessions must commit all work first
- **Merge in dependency order** (check planning document)
- After each merge, check which blocked tasks are now ready
- Merge one branch at a time, testing after each
- Only clean up worktrees after all tests pass

### Skill 2: `/create-example` (Worker Skill)

**File:** `.claude/skills/create-example/SKILL.md`

**Purpose:** Runs inside each worktree to implement the task defined in the spec.

**Key sections to include:**
- **Phase 0: Dependency check** - verify dependencies are merged, stop if not satisfied
- Phase 1: Research - read docs, study existing code
- Phase 2: Proposal - present plan for approval
- Phase 3: Implementation - write code following conventions
- Phase 4: Documentation - record friction points/grievances
- Phase 5: Integration - register, test
- Phase 5.3: **Commit phase - CRITICAL** - commit all work before finishing

**Dependency enforcement:**
```bash
# In Phase 0, check if dependencies are merged
git branch --merged | grep <dependency-branch>

# If not found, STOP and tell user:
# "⚠ BLOCKED: This task depends on X which is not merged yet"
```

**Critical implementation detail:** Worker must commit before notifying user:
```bash
git add .
git commit -m "feat: implement {task} - [description]"
```

Then explicitly tell user: "Work is complete and committed. Ready for review and merge by the orchestrator."

**Customize this skill** for your project's specific task type (examples, features, fixes, etc.).

### Skill 3: `/worktree-help`

**File:** `.claude/skills/worktree-help/SKILL.md`

**Purpose:** Quick reference for worktree commands.

**Key sections to include:**
- Quick commands (`git-worktree-add`, `git-worktree-remove`, `git worktree list`)
- Directory structure diagram
- Common issues and solutions
- Link to full guidelines document

### Skill File Template

```markdown
---
description: "Brief description shown in skill list"
argument-hint: "[arg1] [arg2] - explanation of arguments"
---

# Skill Name

[When to use this skill]

## Workflow

### Phase 1: [Name]
[Steps...]

### Phase 2: [Name]
[Steps...]

## Commands Reference

```bash
# Useful commands for this skill
```

## Example Session

```
User: /skill-name

Claude: [Example interaction...]
```
```

### Creating Skills for a New Project

1. Create the skills directory (adapt to your tool):
   ```bash
   # Claude Code
   mkdir -p .claude/skills/{orchestrate-parallel-tasks,worktree-help}

   # Codex
   mkdir -p .codex/skills/{orchestrate-parallel-tasks,worktree-help}

   # Other tools: use your tool's convention
   ```

2. Copy or create SKILL.md files based on templates above

3. Customize the worker skill for your project type:
   - For a framework tutorial project: `/create-example`
   - For a feature-driven project: `/implement-feature`
   - For a bug-fix workflow: `/fix-issue`

4. Add your tool's config directory to `.worktreesync` so skills are available in worktrees:
   ```
   # Claude Code
   .claude

   # Codex
   .codex

   # Add other tool configs as needed
   ```

5. Test by running the skill: `/orchestrate-parallel-tasks` (or your tool's equivalent)

## Future Improvements

- [ ] Auto-cleanup of stale worktrees
- [ ] Git hook for automatic sync on worktree creation
- [ ] Integration with `direnv` for environment variables
- [ ] Branch deletion option in remover command

---

*Version 0.2 - 2026-02-08*
