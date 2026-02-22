# Worktree Environment Setup

Optional setup for parallel work using git worktrees.

---

## When to Use

**Set up worktrees if you want to:**
- Work on multiple features/bugs in parallel
- Keep main branch always clean and deployable
- Quick context switching without stashing
- Run tests on one branch while coding another

**Skip this setup if:**
- You work on one task at a time
- Your team uses a different branching strategy
- You prefer traditional branch switching

---

## Prerequisites

- Git repository initialized
- Main repository setup complete (see SETUP.md)
- Shell access (bash or zsh)

---

## Setup

### Step 1: Create Worktrees Directory

Create a directory parallel to your repository for worktrees:

```bash
# From your repository root
cd ..
mkdir -p Worktrees/$(basename $(pwd))
cd -
```

**Directory structure after setup:**
```
/path/to/
├── your-repo/                    # Main worktree (main/master branch)
│   ├── .git/
│   ├── src/
│   └── ...
└── Worktrees/
    └── your-repo/                # Additional worktrees go here
        ├── feature-auth/         # Working on auth feature
        ├── bugfix-login/         # Fixing login bug
        └── refactor-api/         # Refactoring API
```

---

### Step 2: Add Shell Functions

Add these functions to `~/.zshrc` or `~/.bashrc`:

```bash
# =============================================================================
# Git Worktree Functions
# =============================================================================

# Create a new worktree with environment sync
git-worktree-add() {
    local WT_PARENT="../Worktrees"
    local REPO_NAME=$(basename $(git rev-parse --show-toplevel))

    if [ ! -d "$WT_PARENT" ]; then
        echo "ERROR: Worktrees directory not found at $WT_PARENT"
        echo "Create it first: mkdir -p $WT_PARENT/$REPO_NAME"
        return 1
    fi

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-add <branch-name>"
        echo "Example: git-worktree-add feature-auth"
        return 1
    fi

    local BRANCH=$1
    local TARGET="$WT_PARENT/$REPO_NAME/$BRANCH"
    local SYNC_FILE=".worktreesync"

    # Create worktree with new branch
    git worktree add "$TARGET" -b "$BRANCH"

    # Sync environment files if .worktreesync exists
    if [ -f "$SYNC_FILE" ]; then
        echo "Syncing environment from $SYNC_FILE..."
        while IFS= read -r item || [ -n "$item" ]; do
            # Skip empty lines and comments
            [[ -z "$item" || "$item" =~ ^# ]] && continue
            # Skip if file/directory doesn't exist
            [ ! -e "$item" ] && continue

            # Create parent directory if needed
            local parent_dir=$(dirname "$TARGET/$item")
            mkdir -p "$parent_dir"

            # Copy file or directory
            cp -r "$item" "$TARGET/$item"
            echo "  + $item"
        done < "$SYNC_FILE"
    fi

    # Initialize submodules if present
    if [ -f "$TARGET/.gitmodules" ]; then
        echo "Initializing submodules..."
        git -C "$TARGET" submodule update --init
    fi

    echo ""
    echo "Worktree created: $TARGET"
    echo "Switch to it: cd $TARGET"
}

# Remove a worktree and clean up
git-worktree-remove() {
    local WT_PARENT="../Worktrees"
    local REPO_NAME=$(basename $(git rev-parse --show-toplevel))

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-remove <branch-name>"
        return 1
    fi

    local BRANCH=$1
    local TARGET="$WT_PARENT/$REPO_NAME/$BRANCH"

    # Check if worktree exists
    if [ ! -d "$TARGET" ]; then
        echo "ERROR: Worktree not found at $TARGET"
        return 1
    fi

    # Remove worktree
    if git worktree remove "$TARGET"; then
        git worktree prune

        # Optionally delete branch
        echo ""
        read -p "Delete branch '$BRANCH'? (y/N) " -n 1 -r
        echo
        if [[ $REPLY =~ ^[Yy]$ ]]; then
            git branch -d "$BRANCH" 2>/dev/null || git branch -D "$BRANCH"
            echo "Branch deleted."
        fi

        echo "Worktree removed: $TARGET"
    else
        echo "ERROR: Could not remove worktree."
        echo "Ensure no terminals are open in that directory."
        return 1
    fi
}

# List all worktrees
git-worktree-list() {
    echo "Worktrees for $(basename $(git rev-parse --show-toplevel)):"
    echo ""
    git worktree list
}

# Quick switch to a worktree
git-worktree-cd() {
    local WT_PARENT="../Worktrees"
    local REPO_NAME=$(basename $(git rev-parse --show-toplevel))

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-cd <branch-name>"
        echo ""
        echo "Available worktrees:"
        ls "$WT_PARENT/$REPO_NAME" 2>/dev/null || echo "  (none)"
        return 1
    fi

    local BRANCH=$1
    local TARGET="$WT_PARENT/$REPO_NAME/$BRANCH"

    if [ -d "$TARGET" ]; then
        cd "$TARGET"
        echo "Switched to: $TARGET"
        echo "Branch: $(git branch --show-current)"
    else
        echo "ERROR: Worktree not found at $TARGET"
        return 1
    fi
}
```

**Reload shell:**
```bash
source ~/.zshrc  # or source ~/.bashrc
```

---

### Step 3: Create .worktreesync File

Create `.worktreesync` in your repository root to specify files that should be copied to new worktrees:

```bash
cat > .worktreesync << 'EOF'
# Environment files (secrets, local config)
.env
.env.local
.env.development.local

# IDE settings
.vscode/settings.json
.idea/

# AI tool configuration (skills available in worktrees)
.claude/
# or: .cursor/, .codex/, .windsurf/

# Specifications (shared across worktrees)
tasks/

# Project documentation
CLAUDE.md
AGENTS.md
EOF
```

**Customize for your project:**
- Add any local config files not in git
- Add your AI tool's configuration directory
- Add specifications directory if you want shared specs
- Remove items you don't need synced

---

### Step 4: Add to .gitignore

Ensure worktree-specific files aren't committed:

```bash
# Add to .gitignore if not already present
echo "" >> .gitignore
echo "# Worktree sync file (contains local paths)" >> .gitignore
echo ".worktreesync" >> .gitignore
```

**Note:** You may want to commit `.worktreesync` if it's useful for your team. Remove from `.gitignore` in that case.

---

### Step 5: Test Setup

```bash
# Create test worktree
git-worktree-add test-worktree

# Verify it was created
git-worktree-list

# Check synced files
ls -la ../Worktrees/$(basename $(pwd))/test-worktree/

# Switch to it
git-worktree-cd test-worktree

# Verify you're in the worktree
pwd
git branch --show-current

# Go back to main repo
cd -

# Remove test worktree
git-worktree-remove test-worktree
```

**Expected output:**
```
Worktrees for your-repo:

/path/to/your-repo              abc1234 [main]
/path/to/Worktrees/your-repo/test-worktree  def5678 [test-worktree]
```

---

## Usage

### Daily Workflow

```bash
# Start new feature
git-worktree-add feature-user-profile

# Switch to feature worktree
git-worktree-cd feature-user-profile

# Work on feature...
# (main repo stays clean)

# When done, go back to main
cd /path/to/main-repo

# Review and merge
git checkout main
git merge feature-user-profile

# Clean up
git-worktree-remove feature-user-profile
```

### Parallel Work

```bash
# Create multiple worktrees
git-worktree-add feature-auth
git-worktree-add bugfix-login
git-worktree-add refactor-api

# Work on each in separate terminals
# Terminal 1: cd to feature-auth
# Terminal 2: cd to bugfix-login
# Terminal 3: cd to refactor-api

# List all active worktrees
git-worktree-list
```

---

## Commands Reference

| Command | Description |
|---------|-------------|
| `git-worktree-add <branch>` | Create new worktree with branch |
| `git-worktree-remove <branch>` | Remove worktree and optionally delete branch |
| `git-worktree-list` | List all worktrees |
| `git-worktree-cd <branch>` | Switch to a worktree directory |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Worktrees directory not found" | Run `mkdir -p ../Worktrees/$(basename $(pwd))` |
| "Could not remove worktree" | Close all terminals open in that worktree directory |
| "Branch already exists" | Use existing branch: `git worktree add ../path <existing-branch>` |
| "Files not syncing" | Check `.worktreesync` exists and has correct paths |
| "Shell functions not found" | Run `source ~/.zshrc` or restart terminal |

---

## Integration with Skills

When using parallel-orchestration skills:

1. **Worktrees enable parallel task execution** - Each task runs in its own worktree
2. **Skills are synced via .worktreesync** - Add your `${TOOL_CONFIG}/` directory
3. **Task specs can be shared or isolated and use a directory** - Choose whether to sync `{TASK_DIR}/` directory

See [skill-setup-guidelines/parallel-orchestration.md](skill-setup-guidelines/parallel-orchestration.md) for parallel work coordination.

---

## Next Steps

After worktree setup:
1. Return to [SETUP.md](SETUP.md) to continue skill installation
2. Consider parallel-orchestration skills if coordinating multiple tasks
3. Update your project documentation with worktree conventions
