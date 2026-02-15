# Skills and Agents Setup Guide

Set up AI coding assistant skills and agents for your repository.

---

## Objective

Enable consistent, reliable AI-assisted development workflows customized to this repository's conventions and needs.

## Key Results

1. Project documentation exists and accurately describes conventions
2. Skills directory exists with at least one working skill
3. User can invoke skill and it follows project conventions
4. All selected skill suites are installed and tested

---

## Setup Checklist

- [ ] 1. Understand repo by reviewing the codebase and interviewing the developer
- [ ] 2. Interview developer to find out what skills and agents to implement
- [ ] 3. Set up the repo with skills and agents (add sub-checklist below)
- [ ] 4. Summarize what was done and confirm with developer

---

## Part 1: Repository Setup

### 1.1 Understand Your Repository Type

Ask yourself these questions to determine your repo type:

**What's the primary purpose?**
- **Prototype/Experiment:** Testing ideas, fast iteration, learning
- **Production Application:** User-facing, high quality and reliability needed
- **Library/Package:** Other code depends on it, API stability matters
- **Tool/CLI:** End users run it directly, UX and error messages matter
- **Internal Tool:** Team uses it, moderate quality standards

**What quality level is appropriate?**
- **Prototype:** Does it work? Is it readable? Document assumptions.
- **Production:** Full quality gates, comprehensive tests, security scans, performance monitoring
- **Library:** API design, complete documentation, semantic versioning, backward compatibility
- **Tool:** User experience, helpful error messages, performance, cross-platform support

**Document your repo type** in CLAUDE.md or equivalent project documentation.

---

### 1.2 Set Up Project Documentation

#### Check for Existing Documentation

```bash
# Look for project documentation
ls CLAUDE.md CONTRIBUTING.md README.md docs/ .github/ 2>/dev/null
```

#### Create CLAUDE.md

If no project documentation exists, create CLAUDE.md with:

**For Prototype Repos:**
```markdown
# Project: [Name]

## Type
Prototype - testing ideas, fast iteration

## Coding Conventions
- Use existing framework patterns
- Keep it simple, avoid over-engineering
- Comment complex logic
- Document assumptions

## Testing
- Tests should pass
- Coverage: >50% (basic confidence)
- Framework: [pytest/jest/etc]

## Quality Standards
- Does it work? ✓
- Is it readable? ✓
- No obvious bugs? ✓
```

**For Production Repos:**
```markdown
# Project: [Name]

## Type
Production Application - user-facing, high quality

## Coding Conventions
[Specific patterns, naming, structure]

## Testing
- Framework: [pytest/jest] with coverage tools
- Coverage: ≥85%
- All PRs must have tests
- Integration tests required

## Quality Standards
- All linting passes ([tool] ≥[score])
- Security scans pass ([tools])
- Performance: [requirements]
- Review: [who reviews, approval process]

## Modularity
- Follow assessment criteria (see references/assessment-approaches.md)
- Complexity <10 per function
- Low coupling, high cohesion
```

**For Library Repos:**
```markdown
# Project: [Name]

## Type
Library/Package - other code depends on it

## API Design
- Semantic versioning (semver)
- Deprecation policy: [details]
- Backward compatibility guarantees

## Testing
- Framework: [tool]
- Coverage: ≥90% (especially public APIs)
- All public APIs must have tests
- Compatibility tests across versions

## Documentation
- All public APIs documented
- Usage examples for all features
- Migration guides for breaking changes
```

#### Create Specs/Plans Folder

```bash
# Detect existing location
for dir in specs dev-docs docs/planning .docs planning; do
  [ -d "$dir" ] && echo "Found: $dir" && break
done

# Or create new
mkdir -p specs/
```

Record the location as `${SPEC_DIR}` in your documentation.

#### Set Up AGENTS.md (Optional)

If you plan to use autonomous agents, create AGENTS.md:

```markdown
# Agent Configuration

## When to Use Agents vs Skills

- **Skills:** Interactive workflows with user approval gates
- **Agents:** Autonomous tasks (code review, security scans, etc.)

## Configured Agents

[List agents and their triggers]

## Approval Workflows

[Define who approves agent actions]
```

---

### 1.3 Set Up Tool Configuration

Detect or create your AI tool's configuration directory:

```bash
# Check for common tool configs
for dir in .claude .cursor .codex .windsurf .ai; do
  if [ -d "$dir" ] || [ -d "$dir/skills" ]; then
    echo "Found tool config: $dir"
    TOOL_CONFIG="$dir"
    break
  fi
done

# If not found, create for your tool
mkdir -p .claude/skills/
mkdir -p .claude/agents/  # If you plan to use autonomous agents
# or .cursor/skills/, .cursor/agents/, etc.
```

**Directory structure:**
- `${TOOL_CONFIG}/skills/` - Skill SKILL.md files defining workflows
- `${TOOL_CONFIG}/agents/` - Agent AGENT.md files for autonomous tasks (optional)

Record the location as `${TOOL_CONFIG}` in your documentation.

---

### 1.4 Set Up Worktrees (Optional)

**Only needed if you want parallel work.** Ask yourself:
"Do I want to run multiple AI sessions in parallel on different tasks?"

If **yes**, set up worktrees:

#### Step 1: Create Worktrees Directory

```bash
# Create directory for worktrees
mkdir ../Worktrees
```

#### Step 2: Add Shell Functions

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
        return 1
    fi

    local NAME=$1
    local BRANCH=$2
    local TARGET="$WT_PARENT/$NAME"
    local INCLUDE_FILE=".worktreesync"

    git worktree add "$TARGET" -b "$BRANCH"

    if [ -f "$INCLUDE_FILE" ]; then
        echo "Syncing environment from $INCLUDE_FILE..."
        while IFS= read -r item || [ -n "$item" ]; do
            [[ -z "$item" || "$item" =~ ^# || ! -e "$item" ]] && continue
            cp -r "$item" "$TARGET/$item"
            echo "  + $item"
        done < "$INCLUDE_FILE"
    fi

    if [ -f "$TARGET/.gitmodules" ]; then
        echo "Initializing submodules..."
        git -C "$TARGET" submodule update --init
    fi

    echo "Successfully created worktree at: $TARGET"
}

# Git Worktree Remove - Cleans up worktree
git-worktree-remove() {
    local WT_PARENT="../Worktrees"

    if [ "$#" -lt 1 ]; then
        echo "Usage: git-worktree-remove <name>"
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

Reload shell: `source ~/.zshrc`

#### Step 3: Create .worktreesync File

```bash
cat > .worktreesync <<'EOF'
# Environment files to sync to worktrees
.env
.env.local

# IDE settings
.vscode/settings.json

# Tool config (so skills are available in worktrees)
.claude
# or .cursor, .codex, etc.
EOF
```

#### Step 4: Test

```bash
git-worktree-add test feat/test
cd ../Worktrees/test
# Verify files synced
ls -la .env .claude/
cd ../../[main-repo]
git-worktree-remove test
```

**For complete worktree guide**, see [skill-setup-guidelines/parallel-orchestration.md](skill-setup-guidelines/parallel-orchestration.md).

---

## Part 2: Choose and Install Skills

### Decision Tree

Determine which skills you need:

1. **Building new features?** → [skill-setup-guidelines/feature-development.md](skill-setup-guidelines/feature-development.md)
2. **Fixing bugs?** → [skill-setup-guidelines/bugfixing.md](skill-setup-guidelines/bugfixing.md)
3. **Refactoring code?** → [skill-setup-guidelines/refactoring.md](skill-setup-guidelines/refactoring.md)
4. **Creating examples/tutorials?** → [skill-setup-guidelines/creating-examples.md](skill-setup-guidelines/creating-examples.md)
5. **Parallel work on large features?** → [skill-setup-guidelines/parallel-orchestration.md](skill-setup-guidelines/parallel-orchestration.md)

**You don't need all skills.** Start with what you need now (usually feature-development).

### Recommended by Repo Type

| Repo Type | Recommended Skills |
|-----------|-------------------|
| Prototype | feature-development (simplified) |
| Production App | feature-development, bugfixing, refactoring |
| Library | feature-development (API focus), refactoring |
| Tool/CLI | feature-development, bugfixing |
| Large Team | All + parallel-orchestration |

### Installation

Each skill guideline document contains complete SKILL.md templates ready to copy:

1. Open the guideline (e.g., `skill-setup-guidelines/feature-development.md`)
2. Find the SKILL.md template sections
3. Copy to `${TOOL_CONFIG}/skills/[skill-name]/SKILL.md`
4. Customize for your repo (see Part 3)

Example:
```bash
# Create skill directory
mkdir -p .claude/skills/feature-define

# Copy template from guideline to SKILL.md
# (Open skill-setup-guidelines/feature-development.md, copy the feature-define template)

# Edit to customize for your repo
```

---

## Part 3: Customize Skills for Your Repo

Skills are **templates** - customize based on your repo type.

### Prototype Repos

**Quality focus:**
- Tests pass (50%+ coverage acceptable)
- Code is readable
- **Skip:** Complexity metrics, full modularity assessments, security scans

**Customize checklist items:**
```markdown
- [ ] 1. Run tests (pytest, coverage >50%)
- [ ] 2. Verify code is readable
- [ ] 3. Document assumptions
- [ ] 4. (Skip for prototype) Security scans
- [ ] 5. (Skip for prototype) Modularity assessment
```

### Production Repos

**Quality focus:**
- Tests pass (85%+ coverage)
- Full modularity assessment (8 criteria from references/assessment-approaches.md)
- Security scans pass (bandit, safety, npm audit, etc.)
- Performance requirements met
- Code review approval

**Customize checklist items:**
```markdown
- [ ] 1. Run tests (coverage ≥85%)
- [ ] 2. Run security scans (bandit, safety)
- [ ] 3. Check performance (<100ms response time)
- [ ] 4. Run modularity assessment (8 criteria)
- [ ] 5. Code review by [tech lead]
- [ ] 6. All checks pass
```

### Library Repos

**Quality focus:**
- Tests pass (90%+ coverage, especially public APIs)
- All public APIs documented
- Semantic versioning compliance
- Backward compatibility tests
- No breaking changes without migration guide

**Customize checklist items:**
```markdown
- [ ] 1. Run tests (coverage ≥90%, focus on public APIs)
- [ ] 2. Verify all public APIs documented
- [ ] 3. Check semver compliance
- [ ] 4. Run backward compatibility tests
- [ ] 5. (If breaking change) Write migration guide
```

### Customization Process

For each skill:

1. **Read the guideline** - Understand phases and structure
2. **Find customization points** - Look for "customize based on repo type"
3. **Adapt quality thresholds** - Match your CLAUDE.md standards
4. **Update Phase 0** - Reference your project docs (CLAUDE.md, CONTRIBUTING.md)
5. **Adjust checklists** - Add/remove items based on repo needs

---

## Part 4: Framework Concepts (Reference)

These concepts underpin the skills framework.

### Core Conventions

#### Progress Tracking Pattern

Skills report progress as they work using:

1. **State variable** - Tracks current phase
2. **Status log** - Audit trail of completed work
3. **Visual progress indicator** - Shows where in the workflow

**Why:** Less likely to skip steps, creates audit trail, provides user visibility.

**Example:**
```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
🔄 Phase 3: Write tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Implement feature
⏸️ Phase 5: Run tests
⏸️ Phase 6: Update documentation
```

#### Documentation Maintenance Pattern

Implementation skills MUST:
1. **Phase 0:** Read documentation (CLAUDE.md, READMEs) before starting
2. **During:** Follow conventions found in documentation
3. **Final Phase:** Update documentation after completing work

**Why:** Prevents code duplication, enforces conventions, keeps docs current.

#### Naming Conventions

- **Skills:** `{task}-{action}` (e.g., `feature-impl`, `bugfix-impl`)
- **Workflows:** `{task}-workflow` (e.g., `feature-workflow`)
- **Reviews:** `{task}-review` (e.g., `code-review`)
- **Exception:** Content creation uses `create-{noun}` (e.g., `create-example`, `create-tutorial`)

### Repository Detection

How to detect existing conventions automatically:

1. **Spec directory** - Check for: specs/, dev-docs/, docs/planning/, .docs/, planning/
2. **Tool config** - Check for: .claude/, .cursor/, .codex/, .windsurf/, .ai/
3. **Project conventions** - Read: CLAUDE.md, README.md, CONTRIBUTING.md
4. **Existing artifacts** - Find PRDs, plans to understand format and style
5. **Codebase structure** - Identify language, framework, test location

### Interview Protocol

How skills customize themselves for your repo:

**Phase 1: Analyze Repository**
- Project type (language, framework)
- Naming conventions (files, classes, functions)
- Documentation patterns
- Testing conventions
- Existing artifacts

**Phase 2: Interview User**
- Confirm detection results
- Approval process (who reviews?)
- Review gates (where to stop for approval?)
- Quality preferences (test coverage, focus areas)
- Tool configuration

**Phase 3: Customize**
- Document output format
- Review gates
- Quality standards
- Workflow preferences

### Prerequisites Validation

Before creating skills, validate:

1. **Spec directory exists:** `mkdir -p ${SPEC_DIR}`
2. **Tool skills directory exists:** `mkdir -p ${TOOL_CONFIG}/skills`
3. **File access works:** Test write permissions
4. **Project documentation accessible:** Can read CLAUDE.md, README.md
5. **Skill consumers identified:** Who/what will use these skills?

---

## Examples

### Example 1: Production Web App Setup

```bash
# 1. Create CLAUDE.md
cat > CLAUDE.md <<'EOF'
# Project: E-commerce Platform

## Type
Production Application - user-facing, high quality

## Conventions
- TypeScript with strict mode
- React components in src/components/
- Tests in __tests__/ alongside source
- Use existing design system components

## Testing
- Jest + React Testing Library
- Coverage: ≥85%
- All PRs must have tests
- Integration tests for critical flows

## Quality Standards
- ESLint + Prettier (no warnings)
- No console.logs in production code
- Performance: <200ms page load
- Security: No XSS, CSRF, SQL injection vulnerabilities
- Review: Tech lead + 1 engineer approval
EOF

# 2. Create specs folder
mkdir -p specs/

# 3. Create tool config
mkdir -p .claude/skills/

# 4. Install feature-development skills
# Copy SKILL.md templates from skill-setup-guidelines/feature-development.md
# into .claude/skills/feature-define/, feature-plan/, feature-impl/

# 5. Customize for production quality
# Edit each SKILL.md:
# - Set coverage threshold to 85%
# - Add security scan steps
# - Add performance check steps
# - Reference CLAUDE.md in Phase 0
```

### Example 2: Prototype Setup

```bash
# 1. Create CLAUDE.md
cat > CLAUDE.md <<'EOF'
# Project: ML Experiment

## Type
Prototype - testing ideas, fast iteration

## Conventions
- Python 3.10+
- Jupyter notebooks in notebooks/
- Scripts in scripts/
- Document assumptions in comments

## Testing
- Basic pytest for core logic
- Coverage: >50%
- Focus on main algorithms

## Quality Standards
- Does it work? ✓
- Is it readable? ✓
- Document assumptions ✓
- No need for: security scans, performance optimization, full modularity
EOF

# 2. Create specs folder
mkdir -p specs/

# 3. Create tool config
mkdir -p .claude/skills/

# 4. Install only needed skills (probably just feature-development)
# Copy and simplify SKILL.md templates

# 5. Customize for prototype speed
# Edit SKILL.md files:
# - Lower coverage threshold to 50%
# - Remove security scan steps
# - Remove modularity assessment steps
# - Simplify checklists
```

### Example 3: Library Setup

```bash
# 1. Create CLAUDE.md
cat > CLAUDE.md <<'EOF'
# Project: Data Processing Library

## Type
Library/Package - other code depends on it

## API Design
- Semantic versioning (semver strict)
- Deprecation policy: 1 major version warning
- Backward compatibility: no breaking changes in minor versions

## Conventions
- Python 3.8+ for compatibility
- Public APIs in package/__init__.py
- Internal modules prefixed with _

## Testing
- pytest with coverage
- Coverage: ≥90% (especially public APIs)
- Compatibility tests across Python 3.8-3.12
- All public APIs must have doctests

## Documentation
- All public APIs documented with examples
- Usage guide in docs/
- Migration guides for breaking changes
- Changelog follows Keep a Changelog format
EOF

# 2. Create specs and docs folders
mkdir -p specs/ docs/

# 3. Create tool config
mkdir -p .claude/skills/

# 4. Install skills focusing on API quality
# Copy SKILL.md templates with API focus

# 5. Customize for library stability
# Edit SKILL.md files:
# - High coverage threshold (90%)
# - Add API documentation checks
# - Add backward compatibility tests
# - Add semver compliance checks
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Where should specs go?" | Common locations: `specs/`, `dev-docs/`, `docs/planning/` - choose one and document in CLAUDE.md |
| "Do I need all skills?" | No - start with feature-development, add others as needed |
| "Worktree setup failed" | Check `../Worktrees/` exists, shell functions added to `~/.zshrc`, reload shell |
| "Skills not found" | Check `${TOOL_CONFIG}/skills/[skill-name]/SKILL.md` exists |
| "Documentation drift" | Skills read CLAUDE.md in Phase 0 and update in Final Phase - ensure these phases run |
| "Quality checks too strict/loose" | Customize thresholds in SKILL.md checklists based on repo type |
| "Skill doesn't match my workflow" | Skills are templates - modify phases and checklists to fit your process |

---

## Next Steps

After setup:

1. **Test a skill** - Try your first skill (e.g., `/feature-define` if using Claude Code)
2. **Review output** - Check if generated specs/plans match your expected format
3. **Iterate on customization** - Adjust quality thresholds, checklist items based on experience
4. **Document learnings** - Update CLAUDE.md with new conventions discovered
5. **Add more skills** - As needs arise, install additional skills from guidelines

---

## References

- **Skill Guidelines:** [skill-setup-guidelines/](skill-setup-guidelines/) - Complete skill templates and guides
- **Agent Guidelines:** [agent-guidelines/](agent-guidelines/) - Agent setup and configuration
- **References:** [references/](references/) - Framework concepts and best practices
  - [assessment-approaches.md](references/assessment-approaches.md) - Code quality assessment criteria
  - [goals-and-objectives.md](references/goals-and-objectives.md) - Setting goals and measurable OKRs
  - [checklist-management.md](references/checklist-management.md) - Checklist patterns and conditionals
  - [spec-maintenance.md](references/spec-maintenance.md) - Keeping spec/plan documents updated

---

*Part of the skills-and-agents framework - a structured approach to AI-assisted software development.*
