# Repository Setup Guide

Interactive guide for setting up a repository with the skills-and-agents framework.

---

## When to Use

Use this guide when setting up the framework in a new or existing repository.

**This guide helps you:**
- Understand your repository type
- Create project documentation (CLAUDE.md)
- Set up folder structure for specs and skills
- Choose and install appropriate skills
- Customize skills for your repo type
- Test that everything works

---

## Prerequisites

- Repository exists (git initialized)
- You have decided to use AI coding assistant skills
- You have read [SETUP.md](../SETUP.md) overview

---

## Workflow

### Phase 0: Understand Repository Type

**Purpose:** Determine repo type to guide setup decisions throughout the process.

**Interview questions:**

1. **What's the primary purpose of this repository?**
   - Prototype/Experiment - Testing ideas, learning, proof of concept
   - Production Application - User-facing app, business-critical service
   - Library/Package - Reusable code for other developers
   - Tool/CLI - Command-line tool or developer utility
   - Internal Tool - Tool for your team/company

2. **What quality level is appropriate?**
   - **Prototype:** Does it work? Is it readable?
   - **Production:** Full quality gates, security, performance, comprehensive tests
   - **Library:** API design, documentation, backward compatibility, very high test coverage
   - **Tool/CLI:** User experience, error messages, help text, edge cases

**Document your answer:** You'll reference this throughout setup.

**Examples:**
- "This is a prototype for testing a new authentication approach" → Prototype
- "This is our customer-facing web application" → Production
- "This is a React component library we're publishing to npm" → Library
- "This is a CLI tool for our internal deployment process" → Tool

**Gate:** Repository type determined and documented.

---

### Phase 1: Create Project Documentation

**Purpose:** Set up CLAUDE.md with conventions that AI assistants will follow.

**Checklist:**

[See [references/checklist-management.md](../references/checklist-management.md) for checklist patterns]

- [ ] 1. Check if CLAUDE.md exists in repository root
- [ ] 2. (If exists) Review current contents and update with framework conventions
- [ ] 3. (If missing) Create CLAUDE.md from template based on repo type
- [ ] 4. Include: Repo type, coding conventions, testing approach, quality standards, folder structure
- [ ] 5. (If planning to use agents) Create AGENTS.md with agent conventions

**Templates by repo type:**

#### Prototype Repo Template

```markdown
# Project Overview

**Repository Type:** Prototype

This is a prototype repository for [purpose]. The focus is on rapid experimentation and learning.

---

## Quality Standards

**Prototype priorities:**
- Does it work?
- Is the code readable?
- Is the approach validated?

**Quality thresholds:**
- Test coverage: >50% (focus on core logic)
- Skip: Security scans, performance optimization, modularity assessments
- Code review: Optional, informal

---

## Coding Conventions

**Language:** [Python/JavaScript/etc.]

**Style:**
- Follow basic language conventions
- Prioritize clarity over cleverness
- Comment complex logic

**Testing:**
- Basic unit tests for core functionality
- Manual testing acceptable for UI/integration

---

## Folder Structure

- `src/` - Source code
- `tests/` - Test files
- `specs/` - Specs, PRDs, technical plans (${SPEC_DIR})
- `.claude/skills/` - AI assistant skills (${TOOL_CONFIG})

---

## Development Workflow

1. Define what you're testing in specs/
2. Implement quickly
3. Test manually or with basic tests
4. Iterate based on learnings
```

#### Production Repo Template

```markdown
# Project Overview

**Repository Type:** Production Application

This is a production application serving [users/customers]. Quality, security, and reliability are critical.

---

## Quality Standards

**Production requirements:**
- All features must meet quality gates before merging
- Security vulnerabilities must be addressed
- Performance requirements must be met
- Comprehensive test coverage required

**Quality thresholds:**
- Test coverage: ≥85%
- Security: All vulnerabilities addressed (bandit, safety, npm audit)
- Performance: <100ms API response time, <2s page load
- Modularity: Pass all 8 modularity criteria
- Code review: Required for all changes

---

## Coding Conventions

**Language:** [Python/JavaScript/etc.]

**Style:**
- Follow [PEP 8 / Airbnb / Google / Standard]
- Use linter: [pylint / eslint / etc.]
- Type hints required: [mypy / TypeScript / etc.]

**Architecture:**
- [MVC / layered / microservices / etc.]
- Keep business logic separate from framework code
- Use dependency injection for testability

**Testing:**
- Unit tests for all business logic
- Integration tests for API endpoints
- E2E tests for critical user flows
- Use fixtures/factories for test data

**Security:**
- Never commit secrets (.env files excluded)
- Validate all user input
- Use parameterized queries (prevent SQL injection)
- Sanitize output (prevent XSS)
- Keep dependencies updated

---

## Folder Structure

- `src/` - Application source code
- `tests/` - Test files (mirrors src/ structure)
- `specs/` - Specs, PRDs, technical plans (${SPEC_DIR})
- `.claude/skills/` - AI assistant skills (${TOOL_CONFIG})
- `docs/` - User documentation
- `scripts/` - Build/deployment scripts

---

## Development Workflow

1. Write PRD in specs/ (use /feature-define skill)
2. Create technical plan (use /feature-plan skill)
3. Implement with tests (use /feature-impl skill)
4. Run full quality checks (coverage, security, modularity)
5. Code review required
6. Merge to main
```

#### Library Repo Template

```markdown
# Project Overview

**Repository Type:** Library/Package

This is a library published for other developers to use. API design, documentation, and backward compatibility are critical.

---

## Quality Standards

**Library requirements:**
- Public APIs must be stable and well-documented
- Backward compatibility maintained (semver)
- Very high test coverage (especially public APIs)
- Breaking changes require major version bump and migration guide

**Quality thresholds:**
- Test coverage: ≥90% (especially all public APIs)
- API documentation: 100% of public interfaces
- Backward compatibility: All tests pass with previous API
- Semantic versioning: Strict compliance
- Code review: Required for all public API changes

---

## Coding Conventions

**Language:** [Python/JavaScript/etc.]

**Style:**
- Follow [PEP 8 / Airbnb / etc.]
- Public APIs: Clear, consistent naming
- Private APIs: Prefix with _ or keep internal

**Architecture:**
- Clear public/private boundary
- Minimal dependencies (don't bloat consumer projects)
- Support common use cases with simple API

**Documentation:**
- Docstrings for all public functions/classes
- Type hints required
- Usage examples in docstrings
- README with quickstart
- CHANGELOG with all changes

**Testing:**
- Unit tests for all public APIs
- Test edge cases and error conditions
- Test examples from documentation
- Compatibility tests with older versions

---

## Folder Structure

- `src/[package-name]/` - Package source
- `tests/` - Test files
- `specs/` - Specs, API designs (${SPEC_DIR})
- `.claude/skills/` - AI assistant skills (${TOOL_CONFIG})
- `docs/` - API documentation
- `examples/` - Usage examples

---

## Development Workflow

1. Design API in specs/ (consider backward compatibility)
2. Write tests first (TDD for public APIs)
3. Implement feature
4. Document with examples
5. Check compatibility with previous version
6. (If breaking change) Write migration guide
7. Update CHANGELOG
8. Code review required
```

**Save as:** `CLAUDE.md` in repository root

**Gate:** CLAUDE.md created and contains conventions appropriate for repo type.

---

### Phase 2: Create Directory Structure

**Purpose:** Set up folders for specifications and skills.

**Checklist:**
- [ ] 1. Check for existing spec directory (specs/, dev-docs/, docs/planning/)
- [ ] 2. (If exists) Use existing directory and record as ${SPEC_DIR}
- [ ] 3. (If missing) Create specs/ directory
- [ ] 4. Document ${SPEC_DIR} location in CLAUDE.md
- [ ] 5. Create ${TOOL_CONFIG}/skills/ directory (.claude/skills/, .cursor/skills/, etc.)
- [ ] 6. Verify write permissions on both directories

**Commands:**

```bash
# Create specs directory (if needed)
mkdir -p specs/

# Create skills directory (adjust .claude based on your AI tool)
mkdir -p .claude/skills/

# Verify directories exist
ls -la specs/ .claude/skills/
```

**Update CLAUDE.md with paths:**

```markdown
## Directory Structure

- **Specifications:** `specs/` (${SPEC_DIR})
  - PRDs, technical plans, implementation plans
  - Created by planning skills

- **Skills:** `.claude/skills/` (${TOOL_CONFIG})
  - AI assistant skill definitions
  - SKILL.md files that define workflows
```

**Gate:** Directories exist, are writable, and paths documented.

---

### Phase 3: (Optional) Set Up Worktrees for Parallel Work

**Purpose:** Enable working on multiple tasks in parallel using git worktrees.

**Ask yourself:** "Do I plan to work on multiple features/bugs in parallel?"

**If yes, continue. If no, skip to Phase 4.**

#### Why Worktrees?

Traditional branching requires stashing/committing before switching. Worktrees allow:
- Multiple branches checked out simultaneously
- Work on feature A while feature B is in review
- Quick context switching without stashing
- Run tests on one feature while coding another

#### Setup Steps

[See [SETUP.md](../SETUP.md) Part 4 for complete worktree setup]

**1. Create worktrees directory:**

```bash
# Create directory parallel to your repo
cd /path/to/your-repo
cd ..
mkdir Worktrees
```

**Structure:**
```
/path/to/
├── your-repo/              # Main worktree (always on main/master)
└── Worktrees/
    └── your-repo/          # Additional worktrees created here
        ├── feature-1/
        ├── bugfix-2/
        └── refactor-3/
```

**2. Add shell functions to ~/.zshrc or ~/.bashrc:**

```bash
# Git worktree helpers
git-worktree-add() {
  local repo_name=$(basename $(git rev-parse --show-toplevel))
  local branch_name=$1
  local worktree_path="../Worktrees/${repo_name}/${branch_name}"

  git worktree add "$worktree_path" -b "$branch_name"
  echo "Created worktree at: $worktree_path"
}

git-worktree-remove() {
  local branch_name=$1
  local repo_name=$(basename $(git rev-parse --show-toplevel))
  local worktree_path="../Worktrees/${repo_name}/${branch_name}"

  git worktree remove "$worktree_path"
  git branch -d "$branch_name"
  echo "Removed worktree: $worktree_path"
}

git-worktree-list() {
  git worktree list
}
```

**3. Create .worktreesync file in repo root:**

```bash
# .worktreesync - Files to sync across worktrees
.claude/
specs/
CLAUDE.md
AGENTS.md
```

**4. Test worktree setup:**

```bash
# Create test worktree
git-worktree-add test-feature

# Verify it was created
git-worktree-list

# Should show:
# /path/to/your-repo              [main]
# /path/to/Worktrees/your-repo/test-feature  [test-feature]

# Remove test worktree
git-worktree-remove test-feature
```

**Gate:** Worktrees configured and tested (if needed) or skipped.

---

### Phase 4: Choose Initial Skills

**Purpose:** Decide which skills to install based on repo type and needs.

**Decision tree:**

[See [SETUP.md](../SETUP.md) Part 2 for complete decision tree]

#### By Repository Type

**Prototype repos:**
- feature-development (simplified: define → implement, skip planning)
- Skip: bugfixing, refactoring, parallel-orchestration

**Production repos:**
- feature-development (full: define → plan → implement)
- bugfixing (investigate → implement)
- refactoring (plan → implement)

**Library repos:**
- feature-development (with API design focus)
- refactoring (maintain code quality)

**Tool/CLI repos:**
- feature-development (with UX focus)
- bugfixing (user-reported issues)

**Large features (any repo type):**
- Add parallel-orchestration if features can be split into parallel tasks

**Educational content:**
- creating-examples (create code examples and tutorials)

#### Recommended Starting Points

**Minimal setup (prototype):**
- feature-define (write PRD)
- feature-impl (implement directly)

**Standard setup (production):**
- feature-define, feature-plan, feature-impl, feature-workflow
- bugfix-investigate, bugfix-impl, bugfix-workflow

**Comprehensive setup (mature production):**
- All feature-development skills
- All bugfixing skills
- All refactoring skills
- parallel-orchestration (if applicable)

**Document your choices:**

```markdown
## Skills to Install

Based on repo type: [Production]

**Feature development:**
- feature-define
- feature-plan
- feature-impl
- feature-workflow

**Bug fixing:**
- bugfix-investigate
- bugfix-impl
- bugfix-workflow

**Code quality:**
- refactor-plan
- refactor-impl
- refactor-workflow

**Skipped:**
- parallel-orchestration (not needed yet)
- creating-examples (not applicable)
```

**Gate:** Skills chosen and documented.

---

### Phase 5: Install Skills

**Purpose:** Copy skill templates from skill-setup-guidelines/ and save to your repository.

**For each chosen skill:**

**Checklist:**
- [ ] 1. Open skill-setup-guidelines/[category].md in this framework repo
- [ ] 2. Find the SKILL.md template for the skill
- [ ] 3. Copy the complete template
- [ ] 4. Create directory: ${TOOL_CONFIG}/skills/[skill-name]/
- [ ] 5. Save template as SKILL.md in that directory
- [ ] 6. Repeat for all chosen skills

**Example for feature-define:**

```bash
# 1. Create skill directory
mkdir -p .claude/skills/feature-define

# 2. Open skill-setup-guidelines/feature-development.md
# 3. Copy the feature-define SKILL.md template
# 4. Save to .claude/skills/feature-define/SKILL.md
```

**Available skill setup guides:**
- [feature-development.md](feature-development.md) - feature-define, feature-plan, feature-impl, feature-workflow
- [bugfixing.md](bugfixing.md) - bugfix-investigate, bugfix-impl, bugfix-workflow
- [refactoring.md](refactoring.md) - refactor-plan, refactor-impl, refactor-workflow
- [parallel-orchestration.md](parallel-orchestration.md) - parallel-orchestrate, worker skills
- [creating-examples.md](creating-examples.md) - create-example, create-tutorial

**Verify installation:**

```bash
# List installed skills
ls -la .claude/skills/

# Should show directories like:
# feature-define/
# feature-plan/
# feature-impl/
# ...
```

**Gate:** All chosen skills installed in ${TOOL_CONFIG}/skills/

---

### Phase 6: Customize Skills for Repo Type

**Purpose:** Adapt skills to match your repository's quality standards.

**Customization areas:**

[See [SETUP.md](../SETUP.md) Part 3 for detailed customization examples]

#### 1. Coverage Thresholds

**Prototype repos:**
```markdown
- [ ] 2. Run tests (coverage >50%)
```

**Production repos:**
```markdown
- [ ] 2. Run tests (coverage ≥85%)
- [ ] 3. (If coverage <85%) Add tests and return to step 2
```

**Library repos:**
```markdown
- [ ] 2. Run tests (coverage ≥90%, especially all public APIs)
- [ ] 3. (If coverage <90%) Add tests and return to step 2
```

#### 2. Quality Gates

**Prototype repos - Skip these:**
```markdown
- [ ] (Skip for prototype) Security scans
- [ ] (Skip for prototype) Modularity assessment
- [ ] (Skip for prototype) Performance optimization
```

**Production repos - Include all:**
```markdown
- [ ] Run security scans (bandit, safety, npm audit)
- [ ] (If vulnerabilities found) Fix and return to security scan
- [ ] Run modularity assessment (8 criteria)
- [ ] (If modularity issues) Refactor and return to assessment
- [ ] Check performance requirements (<100ms response)
```

**Library repos - Add extra checks:**
```markdown
- [ ] Verify all public APIs documented
- [ ] Check backward compatibility
- [ ] (If breaking changes) Write migration guide and update CHANGELOG
- [ ] Verify semantic versioning compliance
```

#### 3. Phase 0 References

**Update all implementation skills to reference your CLAUDE.md:**

```markdown
### Phase 0: Read Documentation

**Purpose:** Understand project conventions before implementing.

**Read:**
- CLAUDE.md (project conventions, repo type, quality standards)
- ${SPEC_DIR}/[plan-name].md (technical plan for this work)

**Checklist:**
- [ ] 1. Read CLAUDE.md and note conventions
- [ ] 2. Read technical plan
- [ ] 3. Understand quality standards for [prototype/production/library] repo

**Gate:** Conventions understood.
```

#### 4. Checklist Conditions

**Add repo-type conditions to all skills:**

```markdown
- [ ] 1. Always: Run basic tests
- [ ] 2. (If production repo) Run security scans
- [ ] 3. (If production repo) Run modularity assessment
- [ ] 4. (If library) Check API documentation completeness
- [ ] 5. (If library) Verify backward compatibility
```

**Customization checklist for each installed skill:**

- [ ] 1. Update coverage thresholds for repo type
- [ ] 2. Add/remove quality gates based on repo type
- [ ] 3. Update Phase 0 to reference CLAUDE.md
- [ ] 4. Add repo-type conditional items to checklists
- [ ] 5. Verify all placeholders replaced (${SPEC_DIR}, ${TOOL_CONFIG})

**Gate:** All skills customized for repository type.

---

### Phase 7: Test Setup

**Purpose:** Verify skills work correctly in your repository.

**Test checklist:**
- [ ] 1. Try invoking first skill (e.g., /feature-define or skill name)
- [ ] 2. Verify skill can read CLAUDE.md
- [ ] 3. Verify skill can write to ${SPEC_DIR}
- [ ] 4. Check progress tracking displays correctly
- [ ] 5. Verify references to framework docs work
- [ ] 6. Test a simple workflow end-to-end
- [ ] 7. Verify customization for repo type works (correct thresholds, gates)

**Simple test workflow:**

```bash
# Test feature-define skill
# Invoke: /feature-define "Add user profile page"
# Should:
# - Read CLAUDE.md to understand conventions
# - Interview you for requirements
# - Create specs/user-profile-prd.md
# - Show progress tracking

# Verify output
cat specs/user-profile-prd.md
# Should contain PRD with requirements, success criteria, etc.
```

**If issues occur:**
- Skill can't find CLAUDE.md → Check file exists in repo root
- Skill can't write to ${SPEC_DIR} → Check directory exists and is writable
- References broken → Check skill references correct paths
- Wrong thresholds → Review Phase 6 customization

**Gate:** Skills working correctly, test output verified.

---

### Phase 8: Document Setup

**Purpose:** Record what was set up for future reference.

**Update CLAUDE.md with setup details:**

```markdown
## Skills Framework

**Setup information:**
- Installed: 2026-02-15
- Framework: skills-and-agents
- Tool: Claude Code (.claude/)
- Specifications directory: specs/ (${SPEC_DIR})

**Skills installed:**
- feature-define - Write PRD for new features
- feature-plan - Create technical plan from PRD
- feature-impl - Implement feature from plan
- feature-workflow - Orchestrates all three

**Customizations:**
- Repository type: Production
- Coverage threshold: 85%
- Security scans: Enabled (bandit, safety)
- Modularity assessments: Enabled (8 criteria)
- Performance checks: Enabled (<100ms response time)

**Quality gates:**
- All features must pass security, modularity, performance checks
- Test coverage ≥85%
- Code review required before merge
```

**Create setup log (optional):**

```bash
# Save setup details
cat > specs/SETUP-LOG.md << 'EOF'
# Framework Setup Log

## 2026-02-15 - Initial Setup

**Repository analyzed:**
- Type: Production application
- Language: Python
- Testing: pytest
- Current state: Existing codebase, adding skills

**Skills installed:**
1. feature-define, feature-plan, feature-impl, feature-workflow
2. bugfix-investigate, bugfix-impl, bugfix-workflow
3. refactor-plan, refactor-impl, refactor-workflow

**Customizations:**
- Coverage: 85% (production standard)
- Security: bandit + safety
- Modularity: All 8 criteria
- Performance: <100ms API response

**Next steps:**
- Start using /feature-define for new features
- Existing features: document retroactively if needed
- Iterate on skill customization as needed
EOF
```

**Gate:** Setup documented in CLAUDE.md and/or specs/SETUP-LOG.md

---

## Progress Tracking

Track your setup progress:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 REPOSITORY SETUP PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Understand repo type (Production)
✅ Phase 1: Create CLAUDE.md
✅ Phase 2: Create directory structure
⏸️ Phase 3: Worktrees (skipped - not needed)
✅ Phase 4: Choose skills
🔄 Phase 5: Install skills [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Customize skills
⏸️ Phase 7: Test setup
⏸️ Phase 8: Document

CURRENT STATUS:
Installing feature-development skills (3/7 complete)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Summary

After completing this guide, you will have:

✅ **Repository type determined** - Prototype, Production, Library, or Tool
✅ **CLAUDE.md created** - Project conventions and quality standards
✅ **Folders created** - specs/ for documentation, ${TOOL_CONFIG}/skills/ for skills
✅ **(Optional) Worktrees configured** - For parallel work if needed
✅ **Skills installed** - Copied from skill-setup-guidelines/ templates
✅ **Skills customized** - Coverage thresholds, quality gates for your repo type
✅ **Setup tested** - Skills working correctly
✅ **Setup documented** - Recorded in CLAUDE.md for future reference

**Next steps:**
1. Start using skills for your work (/feature-define, /bugfix-investigate, etc.)
2. Iterate on skill customization as you learn what works
3. Add more skills as needs arise (use [create-skill.md](create-skill.md))
4. Update CLAUDE.md as conventions evolve

**Getting help:**
- Skill not working? Check Phase 7 troubleshooting
- Need different quality gates? Revisit Phase 6 customization
- Want to create custom skill? See [create-skill.md](create-skill.md)
- Need to understand framework patterns? See [../references/](../references/)

**Remember:** Skills are tools to help you maintain quality and consistency. Customize them to match your needs, not the other way around.
