# Repository Detection (Phase 0)

Common repository detection process used by all skill setup guides.

---

## Purpose

Before setting up any skill, detect and validate the repository structure. This ensures skills are customized to the project's conventions.

---

## Step 1: Detect Spec Directory

Find where specifications, PRDs, and plans are stored:

```bash
# Common locations
for dir in specs dev-docs docs/planning .docs planning docs/prd; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# If not found, ask user
if [ -z "$SPEC_DIR" ]; then
  echo "Where should specifications be stored? (e.g., specs/, dev-docs/)"
  # Get user input and create directory
fi
```

**Record the spec directory** for use in skill templates.

---

## Step 2: Detect Tool Configuration

Find the AI tool's configuration directory:

```bash
# Check for common tool configs
for dir in .claude .cursor .codex .windsurf .ai; do
  if [ -d "$dir" ] || [ -d "$dir/skills" ]; then
    echo "Found tool config: $dir"
    TOOL_CONFIG="$dir"
    break
  fi
done

# If not found, ask user which tool they're using
if [ -z "$TOOL_CONFIG" ]; then
  echo "Which AI tool configuration directory should be used?"
  # Common options: .claude/, .cursor/, etc.
fi
```

---

## Step 3: Read Project Conventions

Extract project-specific conventions from documentation:

```bash
# Read project documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - Naming conventions (files, classes, functions)
# - Document structure preferences
# - Approval/review processes
# - File naming patterns (e.g., YYYY-MM-DD-feature-name-prd.md)
# - Testing conventions
# - Code style guidelines
```

**Key questions to answer:**
- What naming pattern does the project use for documents?
- Who approves PRDs/plans before implementation?
- What testing framework is used?
- Are there existing patterns to follow?

---

## Step 4: Check for Existing Artifacts

Review existing documents to understand format and style:

```bash
# Find existing PRDs
find ${SPEC_DIR} -name "*-prd.md" -o -name "prd-*.md" 2>/dev/null

# Find existing plans
find ${SPEC_DIR} -name "*-plan.md" -o -name "plan-*.md" 2>/dev/null

# Read 1-2 examples to extract format and style
```

---

## Step 5: Analyze Codebase Structure

Understand the project's code organization:

```bash
# Identify project type and structure
ls -la

# Common patterns to look for:
# - src/, lib/, app/ - source code location
# - tests/, test/, __tests__/ - test location
# - docs/ - documentation
# - requirements.txt, package.json, Cargo.toml - dependencies
# - framework/ - framework submodule (if applicable)
```

**Key questions to answer:**
- What language/framework is this project?
- Where are tests located?
- Is there a framework or shared library?

---

## Output: Detection Summary

After completing detection, document findings:

```markdown
## Repository Detection Summary

- **Spec directory**: [detected or chosen location]
- **Tool config**: [detected tool configuration directory]
- **Document naming pattern**: [detected pattern, e.g., YYYY-MM-DD-feature-name-prd.md]
- **Project type**: [language/framework]
- **Test location**: [detected test directory]
- **Existing artifacts**: [count of PRDs, plans, etc.]
- **Key conventions**: [summary of project conventions]
```

---

## Gate

**Do not proceed to Phase 1 until:**
- Spec directory exists or location is confirmed
- Tool configuration directory is identified
- Project conventions are understood
- At least 1-2 existing documents reviewed (if available)

---

## Usage in Setup Guides

Each skill setup guide should reference this document:

```markdown
## Phase 0: Repository Detection

Follow the standard repository detection process from [shared/repo-detection.md](../shared/repo-detection.md).

**Suite-specific checks:**
- [Any additional checks specific to this skill suite]
```

This ensures consistent detection across all skill setups while allowing suite-specific additions.
