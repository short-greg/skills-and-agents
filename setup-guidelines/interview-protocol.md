# Interview Protocol for Skill Customization

How to interview users and analyze repositories to customize skills for specific projects.

---

## Purpose

Skills should not be copy-pasted as-is. During setup, analyze the repository and interview the user to customize skills for their specific project, conventions, and workflow.

---

## When to Use This Protocol

Use this protocol when:
- Setting up skills for a new project
- Adapting skills to a different codebase
- User requests skill customization
- Existing skills don't match project conventions

---

## Phase 1: Repository Analysis

Before interviewing the user, gather information from the codebase.

### 1.1 Project Type Detection

```bash
# Detect project type from files present
ls package.json      # JavaScript/TypeScript
ls requirements.txt  # Python
ls Cargo.toml        # Rust
ls go.mod            # Go
ls pom.xml           # Java/Maven
ls build.gradle      # Java/Gradle
ls Gemfile           # Ruby
```

**Record:** Primary language, framework, package manager

### 1.2 Naming Convention Detection

```bash
# Read existing code to detect patterns
# Look for:
# - File naming: snake_case.py, camelCase.js, PascalCase.cs
# - Class naming: PascalCase, snake_case
# - Function naming: snake_case, camelCase
# - Variable naming: snake_case, camelCase
```

**Record:** Naming patterns for files, classes, functions, variables

### 1.3 Documentation Convention Detection

```bash
# Find existing documentation
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null

# Look for:
# - Document structure preferences
# - Required sections
# - Formatting conventions
# - Approval processes
```

**Record:** Documentation patterns and requirements

### 1.4 Testing Convention Detection

```bash
# Find test directory
ls tests/ test/ __tests__/ spec/ 2>/dev/null

# Identify test framework
grep -r "pytest\|unittest\|jest\|mocha\|rspec" package.json requirements.txt 2>/dev/null
```

**Record:** Test location, framework, naming convention

### 1.5 Existing Artifacts Analysis

```bash
# Find existing PRDs, plans, specs
find . -name "*-prd.md" -o -name "*-plan.md" -o -name "*-spec.md" 2>/dev/null

# Read 1-2 examples to understand format
```

**Record:** Existing document formats, naming patterns, content structure

---

## Phase 2: User Interview

After repository analysis, ask the user targeted questions.

### 2.1 Confirm Detection Results

Present findings and ask for confirmation:

```markdown
## Repository Analysis Results

Based on my analysis:
- **Project type:** Python with FastAPI
- **Naming conventions:** snake_case for files and functions, PascalCase for classes
- **Test framework:** pytest in tests/ directory
- **Spec directory:** dev-docs/
- **Existing PRDs:** 3 found, using YYYY-MM-DD-feature-name-prd.md format

**Questions:**
1. Is this accurate?
2. Are there any conventions I missed?
3. Should I follow the existing PRD format exactly?
```

### 2.2 Workflow Questions

```markdown
## Workflow Questions

1. **Approval process:** Who approves PRDs/plans before implementation?
   - [ ] Just me (solo developer)
   - [ ] Team lead review
   - [ ] Formal stakeholder approval
   - [ ] Other: ___

2. **Review gates:** Where do you want mandatory review stops?
   - [ ] After PRD creation
   - [ ] After development plan
   - [ ] After implementation (before merge)
   - [ ] All of the above

3. **Parallelization:** Do you want to run tasks in parallel using worktrees?
   - [ ] Yes, for large features
   - [ ] No, sequential is fine
   - [ ] I don't know what this means (explain)
```

### 2.3 Quality Preferences

```markdown
## Quality Preferences

1. **Test coverage:** What's your target?
   - [ ] 80%+ coverage required
   - [ ] Critical paths only
   - [ ] No specific target

2. **Code review focus:** What matters most?
   - [ ] Performance
   - [ ] Security
   - [ ] Maintainability
   - [ ] All equally

3. **Documentation level:** How much documentation?
   - [ ] Minimal (code should be self-documenting)
   - [ ] Moderate (public APIs documented)
   - [ ] Comprehensive (all functions documented)
```

### 2.4 Tool-Specific Questions

```markdown
## Tool Configuration

1. **AI tool:** Which AI coding assistant are you using?
   - [ ] Claude Code
   - [ ] Cursor
   - [ ] Codex
   - [ ] GitHub Copilot
   - [ ] Other: ___

2. **Skills location:** Where should skills be stored?
   - Detected: .claude/skills/
   - [ ] Use detected location
   - [ ] Different location: ___
```

---

## Phase 3: Customization Decisions

Based on analysis and interview, make customization decisions.

### 3.1 Document Output Format

Customize output templates to match project conventions:

```markdown
## Customization Decisions

### Document Format
- PRD naming: [YYYY-MM-DD-feature-name-prd.md]
- Plan naming: [YYYY-MM-DD-feature-name-plan.md]
- Location: [dev-docs/]

### Review Gates
- PRD review: [Yes - user approval required]
- Plan review: [Yes - user approval required]
- Implementation review: [Yes - code review checklist]

### Quality Standards
- Test coverage target: [80%]
- Primary review focus: [Maintainability]
- Documentation level: [Moderate - public APIs]

### Workflow
- Parallelization: [Enabled for large features]
- Approval process: [Solo developer - self-approval]
```

### 3.2 Generate Customized Skills

Create skill files with project-specific values:

```markdown
# Example: Customized feature-define skill

## Output
PRD at `dev-docs/YYYY-MM-DD-feature-name-prd.md` containing:
- [Project-specific sections from existing PRDs]
- [Sections required by project conventions]
```

---

## Interview Output

After completing the interview, produce a summary:

```markdown
## Skill Customization Summary

**Project:** [project-name]
**Date:** YYYY-MM-DD

### Repository Analysis
- Project type: [type]
- Language: [language]
- Framework: [framework]
- Test framework: [framework]

### Conventions Detected
- File naming: [pattern]
- Class naming: [pattern]
- Function naming: [pattern]
- Document naming: [pattern]

### User Preferences
- Approval process: [description]
- Review gates: [list]
- Test coverage: [target]
- Documentation level: [level]

### Customizations Applied
- [Customization 1]
- [Customization 2]
- [Customization 3]

### Skills Created
- [skill-name]: [location]
- [skill-name]: [location]
```

---

## Best Practices

1. **Analyze before asking** - Don't ask questions you can answer from the codebase
2. **Confirm findings** - Always verify detected conventions with the user
3. **Offer sensible defaults** - Users should be able to accept defaults quickly
4. **Explain trade-offs** - When asking preference questions, explain implications
5. **Document decisions** - Record all customization decisions for future reference
6. **Be adaptable** - Not all projects fit standard patterns; be flexible

---

## Anti-Patterns to Avoid

1. **Copy-paste setup** - Don't use skills without customization
2. **Ignoring existing patterns** - Don't introduce new conventions that conflict
3. **Over-interviewing** - Don't ask 50 questions; focus on what matters
4. **Assuming context** - Don't assume conventions without verification
5. **Rigid templates** - Don't force project to match template; adapt template to project
