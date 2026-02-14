# Skills and Agents Setup Guidelines and Conventions

General conventions and principles for creating skills and agents across different AI tools and projects.

---

## Purpose

This document contains **ONLY** general conventions, principles, and quality standards. It does NOT contain specific skill definitions.

**For specific skill implementations**, see the suite setup documents:
- [Orchestration Skill Suite](orchestration-skill-suite-setup.md) - Parallel task management
- [PRD Skill Suite](prd-skill-suite-setup.md) - Requirements documentation
- [Dev Plan Skill Suite](dev-plan-skill-suite-setup.md) - Development planning
- [Implementation Skill Suite](implementation-skill-suite-setup.md) - TDD implementation
- [Example/Tutorial Skill Suite](example-tutorial-skill-suite-setup.md) - Creating examples and tutorials

This document is tool-agnostic and adaptable to:
- Different AI tools (Claude Code, Cursor, Windsurf, etc.)
- Different project structures
- Different team workflows

**Contents:**
- **General principles** for skill/agent creation
- **Naming conventions** and patterns
- **Quality standards** for maintainable skills/agents
- **Anti-patterns** to avoid

---

## Understanding Skills vs Agents

### Skills
Skills are **user-invocable workflows** that guide AI through multi-phase processes.

**Characteristics:**
- Structured, multi-phase workflows
- User-driven (invoked explicitly)
- Process-oriented (how to do something)
- Often disable automatic model invocation to enforce step-by-step execution
- Accept arguments for customization

**Best for:**
- Multi-step processes with defined phases
- Workflows requiring user confirmation at checkpoints
- Tasks with clear start and end states
- Repeatable procedures (feature implementation, debugging, reviews)

### Agents
Agents are **specialized assistants** that AI can delegate to. They run as sub-processes with focused expertise.

**Characteristics:**
- Domain-specific expertise
- Autonomously handle delegated tasks
- Return results to the main conversation
- Invoked by the AI tool, not directly by users
- Can specify model preferences for performance optimization

**Best for:**
- Specialized tasks requiring deep expertise
- Work that can be delegated and returned
- Tasks where consistent methodology matters
- Reducing cognitive load on the main conversation

---

## Orchestration Conventions

When skills operate within an orchestration model (managed by a parent orchestrator skill):

### Task Decomposition Principles

- Each task should be **atomic** and independently testable
- Tasks specify their **dependencies** on other tasks
- Independent tasks can run **in parallel**
- Dependent tasks wait until prerequisites complete

### Task State Tracking Format

Tasks are tracked in a structured format (YAML, JSON, or other). The exact format is defined by the orchestration suite, but should include:

- **Task ID**: Unique identifier
- **Description**: What the task accomplishes
- **Dependencies**: List of task IDs this depends on
- **Status**: Current state (pending, ready, in_progress, completed, failed)
- **Outputs**: Files or artifacts produced

### Task Status Transitions

Standard state machine:
```
pending → ready → in_progress → completed
                                    ↓
                                 failed → ready (retry)
```

### PRD/Plan Authority

**Important**: When a skill runs as part of an orchestrated workflow:
- The PRD/plan may override the skill's default phases
- The plan may define specific implementation approaches
- Task context may constrain scope or method
- Skills must check for and respect these overrides

**See**: [Orchestration Skill Suite](orchestration-skill-suite-setup.md) for implementation details.

---

## Core Principles

### 1. Be Specific About Names and Structure

When creating skills or agents, always specify:

**Skill Names:**
- Use descriptive, action-oriented names
- Follow consistent naming conventions
- Make purpose clear from the name

✅ **Good Examples:**
- `plan-development` - Clear what it does
- `implement-feature-tdd` - Action + methodology clear
- `review-pr` - Specific action
- `debug-production-issue` - Context + action

❌ **Bad Examples:**
- `dev` - Too vague
- `helper` - Doesn't describe what it helps with
- `feature` - Incomplete thought
- `do-stuff` - Not descriptive

**Agent Names:**
- Use role-based or expertise-based names
- Indicate the domain or specialty

✅ **Good Examples:**
- `unit-test-architect` - Role + domain
- `security-reviewer` - Role + specialty
- `api-design-expert` - Expertise + domain
- `performance-optimizer` - Action + domain

❌ **Bad Examples:**
- `agent1` - No meaning
- `smart-agent` - Too generic
- `helper-bot` - Doesn't indicate expertise

### 2. Be General About Tools and Locations

Make guidelines adaptable to different environments:

**Project Structure References:**

✅ **Good (General):**
- "Store specifications in your project's spec directory"
- "Follow your project's documentation convention"
- "Use your tool's configuration directory"

❌ **Bad (Too Specific):**
- "Store in `.claude/skills/`" (tool-specific)
- "Must use `dev-docs/`" (project-specific)
- "Run `claude` command" (tool-specific)

**Tool References:**

✅ **Good (General):**
- "Your AI tool's configuration location"
- "The skill invocation mechanism in your tool"
- "Your tool's sub-agent capability"

❌ **Bad (Too Specific):**
- "Claude Code's `.claude/` directory"
- "Use `/skill-name` syntax"
- "Use the Task tool"

**Configuration Discovery:**

Always include a discovery step:
```markdown
## Phase 0: Environment Detection

1. **Detect spec directory location:**
   - Check for: `dev-docs/`, `specs/`, `docs/planning/`, `.docs/`
   - If not found, ask user: "Where should specs be stored?"

2. **Detect tool configuration:**
   - Check for: `.claude/`, `.cursor/`, etc.
   - Adapt to discovered structure

3. **Detect project conventions:**
   - Read: CLAUDE.md, README.md, CONTRIBUTING.md
   - Extract: naming conventions, workflow, structure
```

### 3. Specify Concrete Skill/Agent Artifacts

**Suite setup documents** (not this file) describe what to create. They must be explicit and specific.

**When creating a suite setup document, specify:**

✅ **Good (Specific) - Example pattern:**
```markdown
## Skills in This Suite

1. **Skill: `[category-action]`**
   - Purpose: [One sentence - what it does]
   - Phases: [X] phases ([list phase names])
   - Output: [What it produces]
   - Arguments: [What it accepts]

2. **Skill: `[category-action]`**
   - Purpose: [One sentence]
   - Phases: [X] phases ([list])
   - Output: [What it produces]
   - Arguments: [What it accepts]
```

❌ **Bad (Vague) - Do NOT do this:**
```markdown
## Skills to Create

1. Planning skill
2. Implementation skill
3. Orchestration skill
```

**For agents:**

✅ **Good (Specific) - Example pattern:**
```markdown
## Agents in This Suite

1. **Agent: `[role-domain]`**
   - Expertise: [Domain and specialization]
   - Input: [What it expects]
   - Output: [What it produces]
   - Model: [Sonnet/Opus/Haiku] ([reasoning])
```

❌ **Bad (Vague) - Do NOT do this:**
```markdown
## Agents to Create

1. Testing agent
2. Security agent
```

---

## Skill Architecture: Three Layers

Skills are organized into three architectural layers, each with a distinct purpose:

### Layer 1: Workflow Orchestrators (User Entry Points)

**Purpose:** High-level skills that orchestrate complete workflows with state detection and review gates.

**Characteristics:**
- User-facing entry points
- Detect current state (what already exists)
- Compose lower-level skills
- Include review gates between phases
- Make execution strategy decisions

**Examples:**
- `feature-workflow` - End-to-end feature development (define → plan → implement)
- `bugfix-workflow` - Hypothesis-based debugging
- `refactor-workflow` - Safe refactoring with modularity focus
- `parallel-orchestrate` - Parallel task coordination

**When to use:**
- User wants guided, end-to-end workflow
- Need state detection and resumability
- Want review gates between major phases
- Appropriate for most feature development

**When NOT to use:**
- User wants direct control of a specific phase
- Working on a single, isolated task
- User is an expert and prefers atomic skills

### Layer 2: Task Routers (Multi-session Coordination)

**Purpose:** Skills that coordinate work across multiple sessions or contexts.

**Characteristics:**
- Route to appropriate implementation based on task type
- Check prerequisites (dependencies, state)
- Delegate to Layer 3 skills
- Don't implement directly

**Examples:**
- `parallel-task-workflow` - Routes to feature-impl, bugfix-impl, or refactor-impl based on task type

**When to use:**
- In parallel worktree sessions
- Need task-type detection and routing
- Coordinating across multiple contexts

### Layer 3: Implementation Skills (Atomic Work Units)

**Purpose:** Atomic units that perform specific implementation work.

**Characteristics:**
- Do one thing well
- Can be composed by higher layers
- Can be used standalone
- No orchestration - pure implementation

**Examples:**
- `feature-define`, `feature-define-review` - PRD creation and review
- `feature-plan`, `feature-plan-review` - Development planning
- `feature-impl` - TDD implementation
- `bugfix-impl` - Hypothesis-based bug fixing
- `refactor-impl` - Safe refactoring

**When to use:**
- User wants direct control
- Called by workflow or router skills
- Standalone quick tasks
- Expert users who know exactly what they need

---

## Workflow Pattern: State Detection

All workflow orchestrators (Layer 1) should detect current state to enable resumability:

```markdown
## Phase 0: Detect Current State

**Check what already exists:**

### Look for PRD
Search for existing requirements:
- Check common locations (specs/, dev-docs/)
- If found: Read it and skip to planning phase

### Look for Plan
Search for development plans:
- Check for *-plan.md files
- If found: Read it and skip to implementation phase

### Look for Partial Implementation
Check if work already started:
- Verify files mentioned in plan
- Check test coverage
- If partial: Resume from where work stopped

**Output:** Starting phase for this workflow
```

**Benefits:**
- Users can resume interrupted work
- No duplicate effort
- Workflows adapt to current state
- Can skip completed phases

---

## Workflow Pattern: Review Gates

Workflows include user approval gates between major phases:

```markdown
## Phase N: Review [Artifact]

**User Review Gate**

Present [artifact] to user:
1. Summarize key points
2. Highlight important decisions
3. Ask: "Does this look good? Any changes needed?"

**If user requests changes:**
- Update artifact based on feedback
- Return to this phase

**If user approves:**
- Proceed to next phase
```

**Benefits:**
- User stays in control
- Catch issues early
- Alignment before proceeding
- Prevents wasted work

---

## Skill Composition: How Skills Use Other Skills

Skills can compose other skills in several ways:

### 1. Direct Delegation (Recommended for Workflows)

The skill explicitly invokes another skill:

```markdown
## Phase 3: Create Development Plan

Invoke the planning skill:
```
/feature-plan <prd-file>
```

Wait for completion, then proceed to Phase 4.
```

**Best for:** Workflow orchestrators that compose multiple skills

### 2. Process Reference (Recommended for Routers)

The skill references the same process another skill follows:

```markdown
## Phase 2: Implement Feature

Follow the TDD implementation process from feature-impl:
1. Design structure
2. Write tests first
3. Implement feature
4. Review modularity

OR directly invoke:
```
/feature-impl <spec-file>
```
```

**Best for:** Task routers that delegate based on type

### 3. Inline Implementation

The skill implements the logic directly (not composing):

```markdown
## Phase 1: Generate Hypotheses

Create at least 3 potential root causes:
[Implementation logic here]
```

**Best for:** Atomic implementation skills (Layer 3)

---

## Naming Conventions

### Workflow-First Naming: `{task}-{step}`

The **task/domain comes first**, then the **step**:

| Pattern | Example | Description |
|---------|---------|-------------|
| `{task}-workflow` | `feature-workflow` | Complete workflow orchestrator |
| `{task}-define` | `feature-define` | Requirements definition |
| `{task}-plan` | `feature-plan` | Planning/design |
| `{task}-impl` | `feature-impl` | Implementation |
| `{task}-review` | `feature-review` | Review/validation |

### Task Categories

Common task types:
- `feature-*` - New feature development
- `bugfix-*` - Bug fixing and debugging
- `refactor-*` - Code refactoring
- `parallel-*` - Parallel task coordination
- `example-*` / `tutorial-*` - Educational content

### Examples

**Workflow orchestrators:**
- `feature-workflow` - Orchestrates feature-define → feature-plan → feature-impl
- `bugfix-workflow` - Orchestrates hypothesis generation → testing → fixing
- `refactor-workflow` - Orchestrates planning → refactoring → verification

**Atomic skills:**
- `feature-define`, `feature-define-review`
- `feature-plan`, `feature-plan-review`
- `feature-impl`, `bugfix-impl`, `refactor-impl`

**Parallel coordination:**
- `parallel-orchestrate` - Main branch orchestrator
- `parallel-task-workflow` - Worker that routes to impl skills
- `parallel-help` - Reference guide

---

## Skill Creation Rules

### Rule 1: Define Clear Phases

Every skill must have well-defined phases:

```markdown
Structure:
- Phase 0: [Optional] Orchestration/Context Setup
- Phase 1: [Always] Input/Understanding
- Phase 2-N: [Core Work] Main workflow steps
- Phase N+1: [Always] Completion/Summary
```

**Phase Requirements:**
- Each phase has a clear purpose
- Each phase has explicit steps
- Each phase has a completion gate
- Phases build on previous phases

### Rule 2: Include Gating Statements

Use explicit gates between phases:

✅ **Good:**
```markdown
## Phase 2: Analysis

[Steps...]

**Gate:** Do not proceed to Phase 3 until user approves analysis.
```

❌ **Bad:**
```markdown
## Phase 2: Analysis

[Steps...]

Now move on to design.
```

### Rule 3: Provide Checklists and Tables

Use structured formats for review and validation:

```markdown
## Code Review Checklist

| Category | Check | Status |
|----------|-------|--------|
| Naming | PascalCase classes | [ ] |
| Naming | snake_case functions | [ ] |
| Quality | No duplication | [ ] |
| Quality | Error handling | [ ] |
```

### Rule 4: Support Orchestration (When Applicable)

If a skill might be used in orchestrated workflows, include Phase 0:

```markdown
## Phase 0: Orchestration Check

If running as part of an orchestrated workflow:
1. Check for orchestration context (task ID, dependencies, etc.)
2. Verify dependencies are met
3. Read and apply any overrides from PRD/plan
4. Mark task as in-progress

If not orchestrated:
- Skip to Phase 1 (standalone execution)
```

### Rule 5: Document Expected Outputs

Clearly specify what each phase produces:

```markdown
## Phase 3: Design

[Steps...]

**Output:**
```markdown
# Design Document

## File Structure
[hierarchy]

## Dependencies
[graph]

## Trade-offs
[analysis]
```

**Format:** Markdown document saved to specs directory
**Filename:** YYYY-MM-DD-feature-name-design.md
```

---

## Agent Creation Rules

### Rule 1: Define Clear Expertise

Every agent must have a specific domain:

```markdown
You are an expert [domain] with deep knowledge of [specialties].

Your expertise includes:
- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]
```

### Rule 2: Specify Methodology

Agents must have a clear process:

```markdown
## Your Analysis Process

Before [output], you must [preparation]:

### Step 1: [Name]
[What to do]

### Step 2: [Name]
[What to do]

### Step 3: [Name]
[What to do]
```

### Rule 3: Define Output Format

Specify exactly what the agent returns:

```markdown
## Output Format

Provide [output type] in this structure:

```[format]
[template with placeholders]
```

### Structure Notes
- [Guidance on section 1]
- [Guidance on section 2]
```

### Rule 4: Include Quality Gates

Agents self-validate before returning:

```markdown
## Quality Checklist

Before finalizing, verify:
- [ ] [Critical criterion 1]
- [ ] [Critical criterion 2]
- [ ] [Convention criterion]
- [ ] [Completeness criterion]

If any check fails, revise before submitting.
```

### Rule 5: Provide Examples

Show what good output looks like:

```markdown
## Example

**Input:**
[Sample input]

**Analysis:**
[What the agent would think about]

**Output:**
```[format]
[Example of good output]
```

**Why this is good:**
- [Reason 1]
- [Reason 2]
```

---

## Adaptation Guidelines

When applying these rules to a specific project:

### Step 1: Discover Project Structure

```markdown
## Project Structure Discovery

1. **Find spec directory:**
   ```bash
   ls -la | grep -E "(dev-docs|specs|docs|planning)"
   ```

2. **Find tool config:**
   ```bash
   ls -la | grep -E "(\\.claude|\\.cursor|\\.windsurf|\\.ai)"
   ```

3. **Read conventions:**
   ```bash
   cat README.md CLAUDE.md CONTRIBUTING.md
   ```

4. **Document findings:**
   - Spec directory: [location]
   - Tool config: [location]
   - Naming conventions: [convention]
   - Workflow: [description]
```

### Step 2: Adapt Templates

Create project-specific templates based on discoveries:

```markdown
# Project-Specific Skill Template

[Based on general template, but with:]
- Correct spec directory path
- Correct tool invocation syntax
- Project-specific conventions
- Project-specific patterns
```

### Step 3: Create Skill/Agent Index

Document what you create:

```markdown
# Skills and Agents Index

## Skills

### `plan-development`
- **Purpose:** [...]
- **Location:** [tool-config]/skills/plan-development/
- **Usage:** [tool-specific syntax]
- **Phases:** [list]

### `implement-feature-tdd`
[...]

## Agents

### `unit-test-architect`
- **Purpose:** [...]
- **Location:** [tool-config]/agents/unit-test-architect.md
- **Invocation:** [how to invoke]
- **Model:** [model choice]
```

---

## Naming Conventions

### Clear Naming Scheme

Skills should follow a consistent, easy-to-understand naming pattern that groups related functionality.

**Pattern:** `{category}-{action}` or `{category}-{action}-{modifier}`

This makes it immediately clear what area of the system the skill operates on.

### Recommended Naming Pattern: Workflow-First

**Pattern:** `{task}-{step}` or `{task}-{step}-{modifier}`

The task/domain comes FIRST, then the step in the workflow. This makes it immediately clear what you're working on.

#### Workflow-Based Naming

| Workflow | Pattern | Examples |
|----------|---------|----------|
| **Feature Development** | `feature-*` | `feature-define`, `feature-plan`, `feature-propose`, `feature-impl`, `feature-review`, `feature-test` |
| **Bug Fixing** | `bugfix-*` | `bugfix-reproduce`, `bugfix-hypothesize`, `bugfix-investigate`, `bugfix-impl`, `bugfix-review` |
| **Refactoring** | `refactor-*` | `refactor-analyze`, `refactor-propose`, `refactor-impl`, `refactor-validate` |
| **Parallel Orchestration** | `parallel-*` | `parallel-orchestrate`, `parallel-plan`, `parallel-setup`, `parallel-status`, `parallel-merge` |
| **Examples/Tutorials** | `example-*`, `tutorial-*` | `example-create`, `example-integrate`, `tutorial-write` |

**Key principles:**
- Task/domain comes FIRST (feature, bugfix, refactor, parallel, example)
- Step comes SECOND (define, plan, impl, review, test)
- Workflows often have orchestrators: `{task}-workflow` that coordinate all steps

**For complete skill suite definitions**, see the individual suite setup documents.

### Legacy Patterns (Less Clear)

These patterns work but are less intuitive than the category-based approach:

**Generic verb-noun pattern:** `{verb}-{noun}[-{modifier}]`

Examples:
- `plan-development` → Better as `plan-dev`
- `implement-feature-tdd` → Better as `impl-feature`
- `review-pr-security` → Better as `review-pr` with security focus
- `debug-production-issue` → Better as `debug-issue`
- `optimize-performance` → Better as `optimize-perf`
- `migrate-database` → Better as `migrate-db`

### Agents

Follow this pattern: `{role}-{domain}` or `{expertise}-{specialty}`

**Examples:**
- `unit-test-architect`
- `security-reviewer`
- `performance-optimizer`
- `api-design-expert`
- `database-migration-specialist`

**Categories:**

| Category | Roles | Examples |
|----------|-------|----------|
| Testing | test-architect, test-generator | `unit-test-architect`, `e2e-test-generator` |
| Security | security-reviewer, vulnerability-scanner | `security-reviewer`, `auth-specialist` |
| Quality | code-reviewer, quality-auditor | `code-reviewer`, `style-auditor` |
| Architecture | design-architect, pattern-expert | `api-design-expert`, `microservice-architect` |
| Performance | performance-optimizer, profiler | `performance-optimizer`, `query-optimizer` |

---

## Documentation Requirements

Every skill and agent must have:

### 1. Purpose Statement

One sentence describing what it does:

```markdown
# [Skill/Agent Name]

[One sentence purpose]

[Optional: 1-2 sentence elaboration]
```

### 2. When to Use

Clear trigger conditions:

```markdown
## When to Use

Use this skill when:
- [Condition 1]
- [Condition 2]
- [Condition 3]

Do NOT use when:
- [Anti-condition 1]
- [Anti-condition 2]
```

### 3. Input/Output Specification

What goes in and what comes out:

```markdown
## Input

Expects:
- [Input 1]: [Description]
- [Input 2]: [Description]

Format: [Description of input format]

## Output

Produces:
- [Output 1]: [Description]
- [Output 2]: [Description]

Format: [Description of output format]
```

### 4. Examples

At least one complete example:

```markdown
## Example

**Scenario:** [Description]

**Input:**
```
[Sample input]
```

**Process:**
1. [What happens]
2. [What happens]

**Output:**
```
[Sample output]
```
```

### 5. Dependencies

What it requires:

```markdown
## Dependencies

### Required
- [Dependency 1]: [Why needed]
- [Dependency 2]: [Why needed]

### Optional
- [Optional dependency]: [Enhancement it provides]
```

---

## Quality Checklist

Before considering a skill or agent complete:

### Skill Quality Checklist

```markdown
- [ ] Clear, descriptive name
- [ ] One-sentence purpose statement
- [ ] Well-defined phases with explicit steps
- [ ] Gating statements between phases
- [ ] Checklists and tables for validation
- [ ] Clear output specifications
- [ ] At least one complete example
- [ ] Handles both standalone and orchestrated modes (if applicable)
- [ ] Documentation for user interaction points
- [ ] Error handling guidance
```

### Agent Quality Checklist

```markdown
- [ ] Clear, descriptive name
- [ ] Expertise statement with specific domain
- [ ] Defined analysis process/methodology
- [ ] Structured output format
- [ ] Self-validation checklist
- [ ] At least one complete example
- [ ] Repository awareness guidance
- [ ] Model preference specified (if relevant)
- [ ] Quality gates defined
- [ ] Edge case handling
```

---

## Common Patterns

### Pattern: Progressive Disclosure

Start simple, add complexity:

```markdown
## Phase 1: Basic Understanding
[Simple overview]

## Phase 2: Detailed Analysis
[Dig deeper]

## Phase 3: Comprehensive Design
[Full complexity]
```

### Pattern: Hypothesis-Driven

Generate options, then validate:

```markdown
## Phase 1: Generate Hypotheses
List at least 3 possible approaches:
1. [Option 1]
2. [Option 2]
3. [Option 3]

Do not proceed until all options are listed.

## Phase 2: Evaluate Options
Test each hypothesis:
[Evaluation criteria]

## Phase 3: Select Approach
Choose based on evaluation.
```

### Pattern: Validation Loop

Check, fix, re-check:

```markdown
## Phase N: Implementation

### Step 1: Implement
[Do the work]

### Step 2: Validate
[Check quality]

### Step 3: Fix Issues
If issues found:
- [Fix process]
- Return to Step 2

If no issues:
- Proceed to next phase
```

---

## Anti-Patterns to Avoid

### ❌ Vague Instructions

**Bad:**
```markdown
## Phase 2: Do the thing

Do the thing well.
```

**Good:**
```markdown
## Phase 2: Analyze Requirements

1. Extract acceptance criteria from PRD
2. Identify edge cases for each criterion
3. List assumptions that need validation
4. Present findings in table format

| Criterion | Edge Cases | Assumptions |
|-----------|------------|-------------|
| [...]     | [...]      | [...]       |
```

### ❌ Missing Gates

**Bad:**
```markdown
## Phase 2: Analysis
[Steps]

## Phase 3: Implementation
[Steps]
```

**Good:**
```markdown
## Phase 2: Analysis
[Steps]

**Gate:** Do not proceed until user approves analysis.

---

## Phase 3: Implementation
[Steps]
```

### ❌ Tool-Specific Language

**Bad:**
```markdown
Use the Task tool to invoke the explore agent.
Store in `.claude/skills/`.
```

**Good:**
```markdown
Use your AI tool's sub-agent capability to delegate exploration.
Store in your tool's skills configuration directory.
```

### ❌ Assuming Context

**Bad:**
```markdown
Read the spec file and implement it.
```

**Good:**
```markdown
## Phase 1: Locate Specification

1. Check common spec locations:
   - `dev-docs/`
   - `specs/`
   - `docs/planning/`
   - `.docs/`

2. If multiple specs found, ask user which to use

3. Read the specification and extract:
   - Requirements
   - Acceptance criteria
   - Constraints
```

---

## Version Control

Track changes to skills and agents:

```markdown
# Skill/Agent Version History

## v1.2.0 - 2026-02-11
- Added Phase 0 for orchestration support
- Enhanced code review checklist
- Added performance validation

## v1.1.0 - 2026-01-15
- Added manual testing phase
- Improved acceptance criteria validation
- Fixed issue with test ordering

## v1.0.0 - 2025-12-01
- Initial version
- 5 core phases
- Basic TDD workflow
```

---

## Testing Your Skills and Agents

### Skill Testing

1. **Test with simple input:** Start with straightforward case
2. **Test phase gates:** Verify progression is enforced
3. **Test with complex input:** Edge cases, ambiguities
4. **Test orchestrated mode:** If applicable, test with task dependencies
5. **Test error paths:** What happens when things go wrong?

### Agent Testing

1. **Test with typical input:** Standard use case
2. **Test with edge cases:** Unusual or boundary inputs
3. **Test quality gates:** Verify self-validation works
4. **Test output format:** Matches specification
5. **Test with multiple invocations:** Consistency check

---

## Maintenance

### When to Update

- Project conventions change
- New patterns emerge
- User feedback reveals gaps
- Edge cases discovered
- Tool capabilities change

### Update Process

1. Document the issue or gap
2. Propose the change
3. Test the updated version
4. Update version number
5. Document in change log

---

## Summary

**Be Specific About:**
- Skill and agent names (exact names, clear purposes)
- Phases and steps (explicit instructions)
- Outputs and formats (precise specifications)
- Quality criteria (measurable standards)

**Be General About:**
- Tool-specific features (adapt to any AI tool)
- Directory structures (detect project conventions)
- Command syntax (use tool-agnostic language)
- Implementation details (focus on principles)

**Always Include:**
- Clear purpose statement
- Well-defined phases/methodology
- Gating statements
- Validation checklists
- Complete examples
- Quality gates

This balance of specificity and generality makes skills and agents:
- **Portable** across tools and projects
- **Maintainable** as conventions evolve
- **Understandable** to new team members
- **Effective** at guiding implementation

---

## Suite Setup Documents

This document contains ONLY conventions and principles. For specific skill implementations, see:

### Available Suites

#### Layer 1: Workflow Orchestrators

1. **[Workflow Suite](workflow-skill-suite-setup.md)** ⭐ **Start here for most users**
   - Purpose: End-to-end workflows with state detection and review gates
   - Skills: feature-workflow, bugfix-workflow, refactor-workflow
   - Use when: Starting a new feature, debugging systematically, or refactoring
   - Note: Composes skills from other suites

#### Layer 2: Parallel Coordination

2. **[Orchestration Suite](orchestration-skill-suite-setup.md)**
   - Purpose: Parallel task management with Git worktrees
   - Skills: parallel-orchestrate, parallel-help, parallel-task-workflow
   - Use when: Breaking large features into parallel, independent tasks
   - Note: parallel-task-workflow routes to implementation skills

#### Layer 3: Atomic Implementation Skills

3. **[Feature Definition Suite](prd-skill-suite-setup.md)**
   - Purpose: Define feature requirements (PRDs) and technical specifications
   - Skills: feature-define, feature-define-review
   - Use when: Just need PRD creation (usually called by workflows)

4. **[Feature Planning Suite](dev-plan-skill-suite-setup.md)**
   - Purpose: Development planning and task decomposition
   - Skills: feature-plan, feature-plan-review
   - Use when: Just need planning (usually called by workflows)

5. **[Feature Implementation Suite](implementation-skill-suite-setup.md)**
   - Purpose: TDD-based implementation with modularity evaluation
   - Skills: feature-impl, bugfix-impl (hypothesis-based), refactor-impl
   - Use when: Direct implementation without workflow orchestration

6. **[Example/Tutorial Suite](example-tutorial-skill-suite-setup.md)**
   - Purpose: Creating framework examples and tutorials
   - Skills: example-create, tutorial-create
   - Use when: Building educational content or framework examples

### Creating New Suites

When creating a new suite setup document:

1. **Follow the Suite Setup Pattern**:
   - Phase 0: Repository Detection
   - Phase 1: Validate Prerequisites
   - Phase 2: Define Skills in Suite
   - Phase 3: Provide SKILL.md Templates
   - Phase 4: Integration Instructions
   - Phase 5: Testing & Validation

2. **Adhere to these conventions** (this document)
3. **Resolve repository-specific uncertainties** (spec directories, tool config)
4. **Provide complete, copy-paste ready templates**

---

## Document Maintenance

**Last Updated:** 2026-02-12

**When to Update This Document:**
- New general conventions emerge
- Naming patterns evolve
- Quality standards change
- Anti-patterns discovered

**When NOT to Update This Document:**
- Adding specific skills (those go in suite setup docs)
- Tool-specific instructions (keep this tool-agnostic)
- Project-specific patterns (those go in CLAUDE.md or similar)

---
