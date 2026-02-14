# Shell Functions for Git Worktrees

Helper functions for creating and managing Git worktrees used by parallel orchestration skills.

---

## Purpose

Git worktrees allow multiple working directories from the same repository, each on a different branch. These shell functions simplify worktree creation with environment file syncing.

---

## Prerequisites

### Directory Structure

Use a `-base` parent folder structure:

```
project-base/
├── project/              # Main repo
│   ├── .worktreesync     # Files to sync to worktrees
│   └── specs/            # Specifications directory
└── Worktrees/            # Worktree directory
```

### Setup

```bash
# If starting fresh
mkdir my-project-base
cd my-project-base
git clone <repo-url> my-project
mkdir Worktrees

# If you have an existing repo
mkdir my-project-base
mv my-project my-project-base/
cd my-project-base
mkdir Worktrees
```

---

## Bash/Zsh Functions

Add to `~/.zshrc` or `~/.bashrc`:

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
source ~/.zshrc  # or source ~/.bashrc
```

---

## PowerShell Functions

Add to your PowerShell profile (`$PROFILE`):

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

---

## The .worktreesync File

Create a `.worktreesync` file in your project root to specify files that should be copied to new worktrees (since Git only tracks indexed files):

```
# Environment files
.env
.env.local

# IDE settings
.vscode/settings.json

# Any other untracked local config needed for development
```

**Note:** Add `.worktreesync` to your `.gitignore` if it contains sensitive paths.

---

## Usage

### Creating a Worktree

```bash
# From your main project directory
git-worktree-add my-feature feat/my-feature

# Start your AI tool in the new worktree
cd ../Worktrees/my-feature
claude  # or your AI tool's command
```

### Removing a Worktree

```bash
# Exit your AI tool and cd out of the worktree first!
cd /path/to/main-project
git-worktree-remove my-feature
```

### Listing Worktrees

```bash
git worktree list
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Worktrees folder does not exist" | Run `mkdir ../Worktrees` |
| "Could not remove worktree" | Close all terminals in that directory first |
| Missing `.env` in worktree | Add `.env` to `.worktreesync` and recreate |
| Branch already exists | Use existing branch: `git worktree add ../Worktrees/name existing-branch` |
| Shell functions not found | Check shell config was reloaded: `source ~/.zshrc` |

---

## Verification

After installation, verify the functions work:

```bash
type git-worktree-add  # Should show the function definition
```
