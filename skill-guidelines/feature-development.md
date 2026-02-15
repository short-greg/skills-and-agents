# Feature Development - Complete Skill Suite

Consolidated setup guide for all feature-related skills: orchestration, requirements definition, development planning, and implementation.

---

## Overview

The **Feature Development Suite** provides a complete workflow from feature idea to tested implementation. This guide contains setup instructions for all related skills in one place.

### Skills in This Suite

| Skill | Purpose | Type |
|-------|---------|------|
| `feature-workflow` | End-to-end orchestration from idea to code | Orchestrator |
| `feature-define` | Create PRD from feature ideas | Atomic |
| `feature-define-review` | Review and validate PRD | Review |
| `feature-plan` | Transform PRD into development plan | Atomic |
| `feature-plan-review` | Review and validate development plan | Review |
| `feature-impl` | Implement feature using TDD | Atomic |

### When to Use Each Skill

**Use `feature-workflow` when:**
- Starting a new feature from scratch
- Want complete orchestration with review gates
- Unsure what already exists (it detects and resumes)

**Use `feature-define` when:**
- Have a feature idea that needs documentation
- Need to create PRD before planning
- Want to align stakeholders on requirements

**Use `feature-plan` when:**
- Have an approved PRD
- Need technical design and task breakdown
- Want to identify parallel work opportunities

**Use `feature-impl` when:**
- Have an approved development plan
- Ready to write code using TDD
- Implementing sequentially (not in parallel)

**Use review skills when:**
- Need validation before proceeding
- Want feedback on completeness
- Ensuring quality standards

### How Skills Work Together

```
feature-workflow (orchestrator)
    │
    ├─► Phase 0: Detect current state
    │       └─► Checks for existing PRD/plan/code
    │
    ├─► Phase 1: Repository-specific interview
    │       └─► Gathers evaluation criteria and thresholds
    │
    ├─► Phase 2: feature-define
    │       └─► Creates PRD
    │
    ├─► Phase 3: User review gate
    │       └─► (Optional: feature-define-review for structured feedback)
    │
    ├─► Phase 4: feature-plan
    │       └─► Creates development plan from PRD
    │
    ├─► Phase 5: User review gate
    │       └─► (Optional: feature-plan-review for structured feedback)
    │
    ├─► Phase 6: Decide execution (parallel vs sequential)
    │
    ├─► Phase 7-8: Propose & review implementation approach
    │
    ├─► Phase 9: feature-impl
    │       └─► Implements with TDD, produces working code
    │
    └─► Phase 10: Review modularity
            └─► Verify against Phase 1 thresholds
```

---

## Phase 0: Repository Detection

Follow the standard repository detection process to understand your project structure.

### Reference: setup-guidelines/repo-detection.md

The repository detection process helps customize skills to your project. Key steps:

1. **Detect Spec Directory** - Where PRDs and plans are stored
2. **Detect Tool Configuration** - AI tool's config directory
3. **Read Project Conventions** - Naming, approval processes, testing
4. **Check Existing Artifacts** - Review existing PRDs/plans for format
5. **Analyze Codebase Structure** - Understand code organization

### Feature Suite Specific Checks

```bash
# Find spec directory
for dir in specs dev-docs docs/planning .docs planning docs/prd; do
  if [ -d "$dir" ]; then
    echo "Found spec directory: $dir"
    SPEC_DIR="$dir"
    break
  fi
done

# Find existing PRDs and plans
find ${SPEC_DIR} -name "*-prd.md" -o -name "prd-*.md" 2>/dev/null
find ${SPEC_DIR} -name "*-plan.md" -o -name "plan-*.md" 2>/dev/null

# Check for test framework
ls -la tests/ test/ __tests__/ 2>/dev/null

# Review project conventions
cat CLAUDE.md README.md CONTRIBUTING.md 2>/dev/null
```

### Detection Summary Template

```markdown
## Feature Suite Detection Summary

- **Spec directory**: ${SPEC_DIR}
- **Tool config**: ${TOOL_CONFIG}
- **Document naming pattern**: [e.g., YYYY-MM-DD-feature-name-prd.md]
- **Project type**: [language/framework]
- **Test location**: [detected test directory]
- **Test framework**: [e.g., pytest, jest, cargo test]
- **Existing PRDs**: [count]
- **Existing plans**: [count]
- **Key conventions**: [summary]
```

**Gate:** Do not proceed until spec directory and tool config are identified.

---

## Phase 1: Prerequisites

Follow the standard prerequisites validation to ensure all required directories and access exist.

### Reference: setup-guidelines/prerequisites.md

Standard validation steps:

1. **Verify Spec Directory Exists** - Create if needed
2. **Verify Tool Skills Directory Exists** - Create base structure
3. **Test File Access** - Ensure read/write permissions
4. **Verify Project Documentation Access** - Can read conventions
5. **Identify Skill Consumers** - Who/what uses these skills

### Feature Suite Specific Prerequisites

```bash
# Create spec directory if needed
mkdir -p ${SPEC_DIR}

# Create skill directories
mkdir -p ${TOOL_CONFIG}/skills/feature-workflow
mkdir -p ${TOOL_CONFIG}/skills/feature-define
mkdir -p ${TOOL_CONFIG}/skills/feature-define-review
mkdir -p ${TOOL_CONFIG}/skills/feature-plan
mkdir -p ${TOOL_CONFIG}/skills/feature-plan-review
mkdir -p ${TOOL_CONFIG}/skills/feature-impl

# Verify access
touch ${SPEC_DIR}/.test-access && rm ${SPEC_DIR}/.test-access
touch ${TOOL_CONFIG}/skills/.test-access && rm ${TOOL_CONFIG}/skills/.test-access
```

### Prerequisites Checklist

```markdown
## Feature Suite Prerequisites

- [ ] Spec directory exists and is writable: ${SPEC_DIR}
- [ ] All skill directories created under ${TOOL_CONFIG}/skills/
- [ ] File access verified
- [ ] Project documentation accessible (CLAUDE.md or README.md)
- [ ] Test framework identified
- [ ] Dependencies installed
```

**Gate:** Do not proceed until all directories exist and are accessible.

---

## Phase 2: Skill Definitions

Complete SKILL.md templates for all skills in the feature development suite.

---

## Skill 1: feature-workflow

**Purpose:** End-to-end feature development orchestrator with state detection and review gates.

**File:** `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md`

```markdown
---
name: feature-workflow
description: End-to-end feature development workflow with state detection and review gates
disable-model-invocation: true
---

# Feature Workflow

Complete feature development orchestrator from requirements to implementation.

## Phase 0: Detect Current State

**Check what already exists:**

### Look for PRD
Search for existing requirements documents:
- Check ${SPEC_DIR} for *-prd.md files
- If found: Read it and skip to Phase 4 (Plan Implementation)

### Look for Plan
Search for existing development plans:
- Check ${SPEC_DIR} for *-plan.md files
- If found: Read it and skip to Phase 6 (Decide Execution)

### Look for Partial Implementation
Check if code mentioned in plan exists:
- If partial: Resume from where work stopped

**Output:** Starting phase for this workflow

---

## Phase 1: Repository-Specific Interview

**Purpose:** Gather evaluation criteria and requirements specific to THIS repository.

**This phase establishes the quality gates and thresholds that will be used throughout the workflow.**

### 1.1: Quality Thresholds

**Ask user:**
- "What test coverage threshold is required for this project?"
  - Examples: 80%, 90%, 100% for critical modules
  - Default if not specified: 80%

- "What code quality score is required?"
  - Python: "What pylint score is required?" (e.g., ≥8.0, ≥9.0)
  - JavaScript: "What eslint configuration should be used?"
  - Default: Follow existing .pylintrc, .eslintrc, or project conventions

- "Are there complexity limits?"
  - Cyclomatic complexity threshold (e.g., <10, <15 per function)
  - Function length limits (e.g., <50 lines)
  - Default: Follow modularity.md recommendations

**Document in:** `${SPEC_DIR}/repository-evaluation-criteria.md` or in skill configuration

---

### 1.2: Security Requirements

**Ask user:**
- "Are there security scanning tools configured for this project?"
  - Python: bandit, safety
  - JavaScript: npm audit, eslint-plugin-security
  - General: git-secrets, trufflehog

- "How should secrets and credentials be handled?"
  - Environment variables? Secret management service?
  - Are there .env files? Vault? AWS Secrets Manager?

- "Are there authentication/authorization requirements for this feature?"
  - Who can access this feature?
  - What permissions are needed?

**Document security requirements for reference during implementation.**

---

### 1.3: Performance Requirements

**Ask user:**
- "Are there response time requirements?"
  - API endpoints: e.g., <100ms, <500ms, <1s
  - UI interactions: e.g., <50ms for critical path
  - Batch operations: e.g., process 1000 items in <5s

- "Are there memory or CPU constraints?"
  - Memory limits per request
  - CPU usage limits

- "Are there database query performance requirements?"
  - Query time limits
  - N+1 query prevention required?
  - Database connection pooling limits?

**If no specific requirements:** Use reasonable defaults and verify during implementation.

---

### 1.4: Approval Process

**Ask user:**
- "Who needs to approve before this feature is complete?"
  - Tech lead? Product owner? Stakeholder?
  - Multiple approvers?

- "What review gates exist?"
  - Code review (PR approval)?
  - Design review?
  - QA testing?
  - Stakeholder demo?

- "Are there deployment gates?"
  - Staging environment testing?
  - Production canary deployment?

**Document the approval workflow to ensure all gates are met.**

---

### 1.5: Repository-Specific Conventions

**Check for:**
- Read CLAUDE.md or CONTRIBUTING.md for project-specific patterns
- Identify test framework (pytest, jest, cargo test, etc.)
- Identify code formatting tools (black, prettier, rustfmt, etc.)
- Identify CI/CD configuration (.github/workflows/, .gitlab-ci.yml, etc.)

**Document findings:**
```markdown
## Repository Evaluation Criteria

**Quality Thresholds:**
- Test coverage: ≥85%
- Pylint score: ≥8.5
- Cyclomatic complexity: <10 per function

**Security Requirements:**
- Security scan: bandit (must pass with 0 high/medium issues)
- Secret handling: Environment variables via .env (never commit)
- Authentication: All API endpoints require JWT token

**Performance Requirements:**
- API response time: <100ms (95th percentile)
- Database queries: <50ms per query
- N+1 prevention: Required (use eager loading)

**Approval Process:**
- Code review: Tech lead approval required
- Testing: QA team validation required
- Demo: Stakeholder sign-off required

**Tools:**
- Testing: pytest with pytest-cov
- Linting: pylint, black
- Security: bandit, safety
- CI/CD: GitHub Actions (.github/workflows/test.yml)
```

---

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-WORKFLOW PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Detect current state
🔄 Phase 1: Repository-specific interview [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Define requirements
⏸️ Phase 3: Review requirements
⏸️ Phase 4: Plan implementation
⏸️ Phase 5: Review plan
⏸️ Phase 6: Decide execution strategy
⏸️ Phase 7: Propose implementation
⏸️ Phase 8: Review proposal
⏸️ Phase 9: Implement
⏸️ Phase 10: Review modularity

CURRENT TASK:
Phase 1: Gathering repository-specific evaluation criteria
Status: Interviewing user for quality thresholds and requirements
Started: [TIME]

CHECKLIST:
✅ Quality thresholds defined
✅ Security requirements documented
🔲 Performance requirements gathered
🔲 Approval process confirmed
🔲 Repository conventions identified
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Output:** `${SPEC_DIR}/repository-evaluation-criteria.md` with all thresholds and requirements documented

**Gate:** User confirms all requirements gathered correctly

---

**Why This Matters:**

These criteria will be used to evaluate:
- feature-impl completion (Phase 9: Assess Quality)
- Test coverage thresholds
- Security scan requirements
- Performance benchmarks
- Approval gates

**Defining them upfront prevents:**
- Moving goalposts during implementation
- Subjective "looks good" evaluations
- Missing critical requirements
- Rework due to unclear standards

---

## Phase 2: Define Requirements

**Only execute if no PRD exists.**

Create the Product Requirements Document:
```
/feature-define
```

**Output:** PRD file in ${SPEC_DIR}/

---

## Phase 3: Review Requirements

**User Review Gate**

Present the PRD to the user:
1. Read the generated PRD
2. Summarize key requirements
3. Ask: "Does this PRD capture the requirements correctly?"

**If changes requested:** Update and return to this phase
**If approved:** Proceed to Phase 4

---

## Phase 4: Plan Implementation

**Only execute if no plan exists.**

Create the development plan from PRD:
```
/feature-plan <prd-file>
```

**Output:** Plan file in ${SPEC_DIR}/

---

## Phase 5: Review Plan

**User Review Gate**

Present the plan:
1. Summarize task breakdown
2. Show dependencies
3. Explain test strategy
4. Ask: "Does this plan look good?"

**If changes requested:** Update and return to this phase
**If approved:** Proceed to Phase 6

---

## Phase 6: Decide Execution Strategy

**Analyze the plan to determine execution approach.**

Check for parallelization opportunities:
- Can tasks be done independently?
- Are there multiple tasks with no dependencies?
- Are tasks cleanly separated?

**If yes → Parallel execution:**
```
/parallel-orchestrate plan <plan-file>
```
Stop here - parallel execution happens in separate sessions.

**If no → Sequential execution:**
Proceed to Phase 7

---

## Phase 7: Propose Implementation

**Design the implementation approach WITHOUT writing code.**

Present to user:
1. **File structure** - What files will be created/modified
2. **Module dependencies** - Import structure
3. **Key functions** - Function signatures (no implementation)
4. **Test strategy** - What tests will verify behavior

**Output:** Implementation proposal for user review

---

## Phase 8: Review Proposal

**User Review Gate**

Ask:
- "Does this implementation approach make sense?"
- "Any concerns about modularity or architecture?"
- "Should I proceed with implementation?"

**If changes requested:** Revise and return to Phase 7
**If approved:** Proceed to Phase 9

---

## Phase 9: Implement

**Execute the implementation:**

```
/feature-impl <plan-file>
```

Follow TDD implementation process:
1. Research existing code
2. Design structure
3. Write tests first
4. Implement feature
5. Run tests
6. Document

**Note:** Use evaluation criteria from Phase 1 (repository-specific interview) to guide implementation quality standards.

**Output:** Implemented feature with passing tests

---

## Phase 10: Review Modularity

**Evaluate code quality against modularity criteria.**

**Note:** Verify against thresholds from Phase 1 (repository-specific interview) including test coverage, code quality scores, complexity limits, security requirements, and performance benchmarks.

### Cohesion Check
- Does each module have a single, clear purpose?
- Is related functionality grouped together?

### Coherence Check
- Do module boundaries make sense?
- Is the purpose of each file/class clear?

### Coupling Check
- Are dependencies minimal?
- Can modules be tested independently?
- Are interfaces clean and simple?

### Threshold Verification
- Test coverage meets Phase 1 threshold?
- Code quality score meets Phase 1 requirement?
- Complexity within Phase 1 limits?
- Security scans pass Phase 1 requirements?
- Performance meets Phase 1 benchmarks?

**Present findings:**
```
Modularity Assessment:
✓/✗ Cohesion: [explanation]
✓/✗ Coherence: [explanation]
✓/✗ Loose Coupling: [explanation]

Quality Thresholds (from Phase 1):
✓/✗ Test coverage: [actual]% vs [threshold]%
✓/✗ Code quality: [actual score] vs [threshold]
✓/✗ Complexity: [actual] vs [limit]
✓/✗ Security: [scan results]
✓/✗ Performance: [actual] vs [requirement]

Recommendations:
- [any refactoring suggestions]
```

**If modularity issues found:** Ask if user wants to refactor

**Output:** Feature complete with modularity verified

---

## Notes

- This skill orchestrates other skills - it does NOT implement functionality directly
- State detection allows resuming from any phase
- Review gates ensure user alignment before proceeding
- Can switch to parallel execution when appropriate
```

---

## Skill 2: feature-define

**Purpose:** Create lightweight, confirmable PRD from feature ideas with user stories and OKRs.

**File:** `${TOOL_CONFIG}/skills/feature-define/SKILL.md`

```markdown
---
name: feature-define
description: Create lightweight, confirmable PRD from feature ideas with user stories and OKRs
disable-model-invocation: true
argument-hint: "[feature idea or description]"
---

# Feature Define

Create a lightweight Product Requirements Document from a feature idea.

## When to Use

Use when:
- Starting a new feature
- Documenting user requirements
- Creating input for development planning

Do NOT use when:
- Writing technical design (use feature-plan)
- PRD already exists

## Input

- Feature idea (text or file path)
- Optional: UI designs, mockups
- Optional: User feedback or business drivers

## Output

PRD at `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md` containing:
- Background and motivation
- Prioritized user stories
- Outcome-oriented OKRs
- UI/interface design
- Non-technical risks
- Scope boundaries

## Process

### Phase 0: Intake

**Purpose:** Capture the initial feature idea and supporting materials.

1. **Capture Feature Idea**
   - If argument is text, capture it
   - If argument is file path, read it
   - If no argument, ask user to describe

2. **Check for UI Designs**
   - Mockups (Figma, Sketch, images)
   - Wireframes
   - Screenshots to reference

3. **Gather Context**
   - Related features
   - User feedback driving this
   - Business goals

**Gate:** User confirms this captures the idea correctly.

---

## Phase 1: Interview & Gather Requirements

**Purpose:** Conduct comprehensive interview to gather all information needed for a complete PRD.

### 1.1: Feature Context Interview

**Ask user:**

1. **Problem Definition:**
   - "What problem does this feature solve?"
   - "Who currently experiences this problem?"
   - "How are they solving it now (workarounds)?"
   - "Why is solving it important/urgent now?"

2. **Target Users:**
   - "Who are the primary users of this feature?" (specific personas)
   - "Who are the secondary users?" (indirect beneficiaries)
   - "Are there admin/operator roles involved?"
   - "What permissions/roles are needed?"

3. **Success Criteria:**
   - "How will we measure success for this feature?"
   - "What metrics matter?" (usage, engagement, conversion, performance)
   - "What defines 'done' from a product perspective?"

**Document answers for use in background section (Phase 2).**

---

### 1.2: Visual Materials Request

**Ask user:**

1. **UI Mockups/Designs:**
   - "Do you have UI mockups or wireframes?"
     - Figma links
     - Sketch files
     - Adobe XD designs
     - Hand-drawn sketches (photos)

2. **Reference Screenshots:**
   - "Do you have screenshots of similar features to reference?"
   - "Are there competitor features to emulate?"
   - "Are there existing UI patterns in the app to follow?"

3. **User Flow Diagrams:**
   - "Do you have user flow diagrams showing the journey?"
   - "Are there state diagrams for complex workflows?"
   - "Are there interaction patterns documented?"

**Gather all materials:**
- Save mockups to `${SPEC_DIR}/mockups/YYYY-MM-DD-feature-name/`
- Save screenshots to `${SPEC_DIR}/screenshots/YYYY-MM-DD-feature-name/`
- Note links/paths in requirements doc

**If no visual materials provided:**
- "Would you like to describe the UI verbally?" (capture description)
- "Should UI design be deferred to planning phase?" (note as TBD)

---

### 1.3: Scope Clarification Interview

**Ask user:**

1. **Must-Have Requirements:**
   - "What absolutely MUST be included in v1?" (non-negotiable)
   - "What is the minimum viable version?"
   - "What cannot be deferred?"

2. **Should-Have Requirements:**
   - "What should be included if time permits?" (important but not critical)
   - "What adds significant value but isn't essential?"

3. **Nice-to-Have Requirements:**
   - "What could be deferred to v2?" (future enhancements)
   - "What are 'stretch goals'?"

4. **Explicitly Out of Scope:**
   - "What is explicitly NOT included?" (prevent scope creep)
   - "What similar features should we NOT build?"
   - "What edge cases are we explicitly ignoring for v1?"

**Document scope boundaries clearly** to prevent misunderstandings later.

---

### 1.4: User Stories Seed Interview

**Ask user (for each persona identified in 1.1):**

1. **Primary Persona Stories:**
   - "As a [primary persona], what do you want to accomplish with this feature?"
   - "What specific actions will [persona] take?"
   - "What value does [persona] get from this?"

2. **Secondary Persona Stories:**
   - Same questions for secondary users
   - Admin/operator stories if applicable

3. **Story Prioritization:**
   - "Which stories are absolutely critical?" (must-have)
   - "Which stories are important but can be deferred?" (should-have)
   - "Which stories are nice-to-have?" (could-defer)

**Capture raw user story ideas** for Phase 3 (User Stories phase).

---

### 1.5: Context and Constraints

**Ask user:**

1. **Related Work:**
   - "Are there existing features this builds upon?"
   - "Are there features this replaces?"
   - "What features does this integrate with?"

2. **Technical Constraints:**
   - "Are there known technical limitations?" (API limits, browser constraints)
   - "Are there performance requirements?" (response time, throughput)
   - "Are there scalability requirements?" (user count, data volume)

3. **Business Constraints:**
   - "Are there timeline constraints?" (deadlines, releases)
   - "Are there budget constraints?" (development time limits)
   - "Are there legal/compliance requirements?" (GDPR, accessibility, etc.)

**Document all constraints** for reference in PRD and planning.

---

### Interview Summary Document

**Create:** `${SPEC_DIR}/YYYY-MM-DD-feature-name-interview-notes.md`

```markdown
# Feature Interview Notes: [Feature Name]

**Date:** YYYY-MM-DD
**Participants:** [Names]

## Feature Context
- Problem: [Description]
- Current Workaround: [Description]
- Urgency: [Why now]

## Target Users
- Primary: [Persona 1]
- Secondary: [Persona 2]
- Admin: [Persona 3]

## Visual Materials
- Mockups: [Links/paths]
- Screenshots: [Links/paths]
- Flow diagrams: [Links/paths]

## Scope
**Must-Have:**
- [Requirement 1]
- [Requirement 2]

**Should-Have:**
- [Requirement 3]

**Nice-to-Have:**
- [Requirement 4]

**Out of Scope:**
- [Excluded item 1]
- [Excluded item 2]

## User Story Seeds
**Primary Persona:**
- Story seed 1
- Story seed 2

**Secondary Persona:**
- Story seed 3

## Constraints
**Technical:**
- [Constraint 1]

**Business:**
- [Constraint 2]

**Legal/Compliance:**
- [Constraint 3]

## Success Metrics
- [Metric 1]
- [Metric 2]
```

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-DEFINE PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Intake
🔄 Phase 1: Interview & Gather Requirements [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Background
⏸️ Phase 3: User Stories
⏸️ Phase 4: OKRs
⏸️ Phase 5: UI Designs
⏸️ Phase 6: Non-Technical Risks
⏸️ Phase 7: Scope Boundaries
⏸️ Phase 8: Write PRD
⏸️ Phase 9: Review and Confirmation

CURRENT TASK:
Phase 1: Interviewing user to gather requirements and materials
Status: Collecting visual materials and scope clarifications
Started: [TIME]

CHECKLIST:
✅ Feature context interview complete
✅ Visual materials requested
🔲 Scope clarification complete
🔲 User story seeds gathered
🔲 Constraints documented
🔲 Interview summary created
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Gate:** User confirms all questions answered, materials gathered, interview summary accurate

---

### Phase 2: Background and Context

**Purpose:** Establish why this feature matters.

**Note:** Use interview notes from Phase 1.

Write background answering:
- What problem does this solve?
- Who experiences this problem?
- Why is solving it important now?
- What's the current workaround?

Keep to 3-5 paragraphs maximum.

**Gate:** User approves background.

---

### Phase 3: User Stories

**Purpose:** Define who needs what and why.

**Note:** Expand story seeds from Phase 1.4.

1. **Identify Personas**
   - Primary users (main beneficiaries)
   - Secondary users
   - Admin/operator roles

2. **Write Stories**
   Format: **As a [persona], I want [capability], so that [benefit]**

   Good characteristics:
   - Specific persona (not "user")
   - Clear capability (one thing)
   - Tangible benefit (outcome, not feature)
   - Independent

3. **Prioritize**
   - **Must Have** - Core value
   - **Should Have** - Important but not critical
   - **Nice to Have** - Can defer

**Gate:** User approves user stories.

---

### Phase 4: OKRs (Success Criteria)

**Purpose:** Define outcome-oriented success criteria.

1. **Write Objective**
   One clear statement: "Enable [aspirational goal]"

2. **Write Key Results**
   3-5 measurable outcomes:
   - KR1: [What will be true when done]
   - KR2: [Measurable capability]
   - KR3: [Performance target]

   Good KRs are:
   - Outcome-oriented (what exists, not what we build)
   - High-level (system capabilities)
   - Measurable (can verify objectively)

   Avoid:
   - "Implement X" (implementation detail)
   - "Add settings page" (feature, not outcome)
   - "Write tests" (process, not outcome)

3. **Map to User Stories**
   Verify each "Must Have" story maps to at least one KR.

**Gate:** User approves OKRs.

---

### Phase 5: UI Designs

**Purpose:** Capture or describe the user interface.

**Note:** Reference materials from Phase 1.2.

If UI mockups provided:
- Include links or embed images
- Extract key UI elements
- Note interactions/flows

If no designs, describe:
- Component type (button/form/dashboard)
- Location in app
- Elements and interactions
- States (default, loading, error, success)

For non-UI features (API/backend):
- Entry points
- Data formats

**Gate:** User approves UI description.

---

### Phase 6: Non-Technical Risks

**Purpose:** Identify business, user, and operational risks.

**Note:** Reference constraints from Phase 1.5.

Categories:
- **User Risks:** Adoption, confusion, workflow disruption
- **Business Risks:** Revenue, compliance, reputation
- **Operational Risks:** Support burden, maintenance
- **Timeline Risks:** Dependencies, unknowns

For each risk:
- Description
- Likelihood (Low/Medium/High)
- Impact (Low/Medium/High)
- Mitigation strategy

**Gate:** User acknowledges risks.

---

### Phase 7: Scope Boundaries

**Purpose:** Define explicit out-of-scope items.

**Note:** Use scope clarifications from Phase 1.3.

1. **List What's NOT Included**
   - Features explicitly excluded
   - User types not supported
   - Edge cases deferred
   - Integrations not included

2. **Explain Why**
   Brief rationale for each exclusion.

**Gate:** User confirms scope boundaries.

---

### Phase 8: Write PRD Document

**Purpose:** Compile all sections into formal document.

Create file: `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`

```markdown
# [Feature Name] PRD

**Date:** YYYY-MM-DD
**Status:** Draft

## Background
[From Phase 2]

## User Stories
[From Phase 3]

## OKRs
[From Phase 4]

## UI Design
[From Phase 5]

## Non-Technical Risks
[From Phase 6]

## Out of Scope
[From Phase 7]
```

---

### Phase 9: Review and Confirmation

**Purpose:** Get stakeholder sign-off.

Present complete PRD and ask:
- "Does this capture the feature correctly?"
- "Any gaps or concerns?"
- "Ready to approve for planning?"

If changes needed:
- Update relevant sections
- Return to this phase

If approved:
- Update status to "Approved"
- Ready for `/feature-plan`

---

## Best Practices

### User Stories
- Be specific about personas
- One capability per story
- Focus on outcomes, not features
- Keep stories independent

### OKRs
- Outcome-oriented, not implementation
- High-level system capabilities
- Measurable and verifiable
- Map to user stories

### Scope
- Be explicit about exclusions
- Explain rationale
- Prevents scope creep
```

---

## Skill 3: feature-define-review

**Purpose:** Review and validate PRD for completeness and clarity.

**File:** `${TOOL_CONFIG}/skills/feature-define-review/SKILL.md`

```markdown
---
name: feature-define-review
description: Review and validate PRD for completeness and clarity
disable-model-invocation: true
argument-hint: "[path-to-prd-file]"
---

# Feature Define Review

Review a PRD for completeness, clarity, and quality.

## When to Use

- Reviewing a draft PRD
- Validating before planning starts
- Providing feedback on requirements

## Input

Path to PRD file (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`)

## Output

Review report with:
- Validation results
- Identified gaps
- Actionable feedback
- Approval status

## Process

### Phase 1: Read PRD

Read the PRD file and extract all sections.

### Phase 2: Validate Structure

Check all required sections present:
- [ ] Interview Notes (if exists)
- [ ] Background
- [ ] User Stories
- [ ] OKRs
- [ ] UI Design (if applicable)
- [ ] Risks
- [ ] Out of Scope

### Phase 3: Validate Interview Notes (if exists)

Check interview notes for:
- [ ] Feature context documented
- [ ] Target users identified
- [ ] Success criteria defined
- [ ] Visual materials gathered or noted as TBD
- [ ] Scope boundaries clarified (must-have/should-have/nice-to-have)
- [ ] User story seeds captured
- [ ] Constraints documented

### Phase 4: Validate User Stories

Check user stories are:
- [ ] Specific personas (not "user")
- [ ] Single capability each
- [ ] Outcome-focused benefits
- [ ] Independent
- [ ] Prioritized

### Phase 5: Validate OKRs

Check OKRs are:
- [ ] Outcome-oriented (not implementation)
- [ ] Measurable
- [ ] High-level
- [ ] Map to user stories

### Phase 6: Identify Gaps

Check for:
- Unresolved questions
- Vague requirements
- Missing edge cases
- Undefined behaviors

### Phase 7: Provide Feedback

Generate review report:

```markdown
# PRD Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** Approved / Needs Changes

## Structure Validation
- [x] Interview notes present
- [x] Background present
- [x] User stories present
- [ ] Missing: UI Design section

## Interview Notes Validation
- [x] Feature context complete
- [x] Visual materials documented
- [ ] Issue: Missing constraints documentation

## User Stories Validation
- [x] Specific personas
- [ ] Issue: Story 3 has two capabilities

## OKRs Validation
- [x] Outcome-oriented
- [ ] Issue: KR2 is implementation detail

## Gaps Identified
1. [Gap description]
2. [Gap description]

## Recommendations
1. [Specific recommendation]
2. [Specific recommendation]

## Approval Decision
- [ ] Approved - Ready for planning
- [x] Needs Changes - Address feedback
- [ ] Major Revision - Significant rework needed
```
```

---

## Skill 4: feature-plan

**Purpose:** Transform PRD into comprehensive development plan with task decomposition and system-level OKRs.

**File:** `${TOOL_CONFIG}/skills/feature-plan/SKILL.md`

```markdown
---
name: feature-plan
description: Transform PRD into comprehensive development plan with task decomposition and system OKRs
disable-model-invocation: true
argument-hint: "[path-to-prd or PRD description]"
---

# Feature Plan

Transform a Product Requirements Document into an actionable development plan.

## When to Use

Use when:
- Have an approved PRD
- Need to create technical design
- Breaking work into parallel tasks
- Before implementation begins

Do NOT use when:
- Creating the PRD (use feature-define)
- Already have a development plan
- Implementing (use feature-impl)

## Input

- PRD file path (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`)
- OR PRD description text

## Output

Development plan at `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md` containing:
- Clarified requirements (with separate clarifications document)
- Risk assessment
- Design options with recommendation
- Task breakdown with dependencies
- System-level OKRs
- Implementation roadmap

Additionally creates:
- `${SPEC_DIR}/YYYY-MM-DD-feature-name-clarifications.md` - Comprehensive record of technical interview questions and answers

## Process

### Phase 0: Intake and Context

**Purpose:** Understand the PRD and gather project context.

1. **Read the PRD**
   - If argument is file path, read it
   - If argument is text, capture it
   - If no argument, ask user to provide PRD

2. **Gather Project Context**
   - Read project conventions (CLAUDE.md, README.md)
   - Review similar features in codebase
   - Check framework documentation
   - Identify relevant patterns

3. **Initial Understanding Check**
   - Summarize PRD in 2-3 sentences
   - List key requirements
   - Note immediate questions

**Gate:** User confirms understanding is correct.

---

## Phase 1: Question Resolution & Technical Interview

**Purpose:** Identify and resolve ALL ambiguities in requirements BEFORE designing the solution.

### 1.1: Generate Questions from PRD

**Review PRD systematically for ambiguities:**

1. **Scope Questions:**
   - Are there features mentioned without clear boundaries?
   - Is "in scope" vs "out of scope" crystal clear?
   - Are there assumptions that need verification?

2. **Requirements Questions:**
   - Which requirements are must-have vs nice-to-have?
   - Are there conflicting requirements?
   - Are requirements measurable/testable?

3. **Behavior Questions:**
   - What happens when [edge case]?
   - What happens if [error condition]?
   - What happens when [concurrent users]?
   - What are the state transitions?

4. **Integration Questions:**
   - How does this interact with existing systems?
   - What APIs/interfaces are involved?
   - What data flows between components?
   - Are there authentication/authorization requirements?

5. **Constraint Questions:**
   - What are the performance requirements? (response time, throughput)
   - What are the scalability requirements? (users, data volume)
   - Are there compatibility requirements? (browsers, devices, API versions)
   - Are there legal/compliance requirements?

**Document all questions in a structured list.**

---

### 1.2: Check for Better Design Approaches

**Even if requirements seem clear, ask:**

1. **Simplicity Check:**
   - "Is there a simpler approach that achieves the same goal?"
   - "Are we over-engineering this?"
   - "Can we reuse existing patterns instead of creating new ones?"

2. **Pattern Check:**
   - "What existing patterns in the codebase should we follow?"
   - "Have we solved similar problems before?"
   - "Are there industry-standard approaches?"

3. **Architecture Check:**
   - "Does this fit the current architecture?"
   - "Would a different architecture be more appropriate?"
   - "Are we creating technical debt?"

**Document alternative approaches for discussion.**

---

### 1.3: Request Technical Visual Materials

**Ask user for technical diagrams:**

1. **Architecture Diagrams:**
   - "Do you have system architecture diagrams?"
   - "Are there component diagrams showing relationships?"
   - "Are there deployment diagrams?"

2. **Data Flow Diagrams:**
   - "Do you have data flow diagrams?"
   - "Are there sequence diagrams for key interactions?"
   - "Are there state machine diagrams?"

3. **Database Schema:**
   - "Do you have entity-relationship diagrams?"
   - "Are there database schema designs?"
   - "What are the data models?"

**If materials exist:**
- Save to `${SPEC_DIR}/diagrams/YYYY-MM-DD-feature-name/`
- Reference in plan document

**If materials don't exist:**
- Note as "To be designed in Phase 3 (Structural Design Options)"
- May need to create these as part of planning

---

### 1.4: Prioritize and Present Questions

**Group questions by priority:**

#### Critical Questions (Blocking Design)
Questions that MUST be answered before any design can proceed:
- [Question 1]
- [Question 2]

#### Important Questions (Affects Design Options)
Questions that significantly impact which design approach to choose:
- [Question 3]
- [Question 4]

#### Nice to Clarify (Minor Impact)
Questions that would be good to resolve but don't block progress:
- [Question 5]
- [Question 6]

**Present to user in this prioritized format.**

---

### 1.5: Conduct Technical Interview

**Ask user for answers to all questions:**

**For each critical question:**
- Ask the question
- Document the answer
- Ask follow-up clarifications if needed
- Confirm understanding: "So you're saying [restate answer]?"

**For important questions:**
- Same process as critical questions
- May reveal additional design constraints

**For nice-to-clarify questions:**
- Ask if time permits
- Note "TBD" if not critical for v1

---

### 1.6: Document Clarified Requirements

**Create:** `${SPEC_DIR}/YYYY-MM-DD-feature-name-clarifications.md`

```markdown
# Requirements Clarifications: [Feature Name]

**Date:** YYYY-MM-DD
**PRD:** [link to PRD]

## Critical Questions & Answers

### Q1: [Question]
**Answer:** [User's answer]
**Implication:** [How this affects design]

### Q2: [Question]
**Answer:** [User's answer]
**Implication:** [How this affects design]

## Important Questions & Answers

### Q3: [Question]
**Answer:** [User's answer]
**Implication:** [How this affects design options]

## Technical Visual Materials

**Received:**
- Architecture diagram: [path/link]
- Data flow diagram: [path/link]

**To be created:**
- Component diagram (will create in Phase 3)
- Database schema (will create in Phase 3)

## Alternative Approaches Considered

**Approach 1:** [Description]
- Pros: [List]
- Cons: [List]
- User feedback: [Comments]

**Approach 2:** [Description]
- Pros: [List]
- Cons: [List]
- User feedback: [Comments]

## Updated Requirements

Based on clarifications, the following requirements are now clear:
- [Requirement 1]: [Clarified version]
- [Requirement 2]: [Clarified version]
```

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-PLAN PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Intake and Context
🔄 Phase 1: Question Resolution & Technical Interview [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Risk and Complexity Analysis
⏸️ Phase 3: Structural Design Options
⏸️ Phase 4: Task Decomposition
⏸️ Phase 5: System-Level OKRs
⏸️ Phase 6: Detailed Development Plan
⏸️ Phase 7: Write Plan Document

CURRENT TASK:
Phase 1: Conducting technical interview to resolve ambiguities
Status: Gathering answers to critical and important questions
Started: [TIME]

CHECKLIST:
✅ Generated questions from PRD
✅ Checked for better design approaches
✅ Requested technical visual materials
✅ Prioritized questions
🔲 Conducted technical interview
🔲 Documented clarifications
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Gate:** All critical questions answered, clarifications documented

**Why This Matters:**

Resolving questions NOW prevents:
- Design rework (discovering requirements during implementation)
- Multiple design iterations (unclear constraints)
- Implementation delays (blocking questions during coding)
- Scope creep (undefined boundaries)

**Answering questions upfront enables:**
- Accurate design (clear requirements)
- Better options (understanding constraints)
- Realistic estimates (known unknowns resolved)
- Stakeholder alignment (confirmed understanding)

---

### Phase 2: Risk and Complexity Analysis

**Purpose:** Identify technical risks and complexity factors.

**Note:** Reference clarifications from Phase 1.6 to inform risk assessment.

1. **Identify Risks**
   - Technical uncertainty (unfamiliar tech?)
   - Integration risk (multiple systems?)
   - Performance risk (scalability?)
   - Complexity risk (complex algorithms?)
   - Dependency risk (third-party?)

2. **Assess Complexity**
   Rate each component:
   - **Simple:** Straightforward, well-understood
   - **Moderate:** Some complexity, patterns exist
   - **Complex:** Novel approach, unknowns

3. **Research Mitigation**
   For each risk:
   - Research solutions
   - Consider prototypes/spikes
   - Identify fallback approaches

**Output:**
```markdown
## Risk Assessment

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| [Risk 1] | High | Medium | [Strategy] |

## Complexity
- Component A: Simple
- Component B: Complex - requires new approach
```

**Gate:** User reviews risk assessment.

---

### Phase 3: Structural Design Options

**Purpose:** Propose multiple design options with trade-offs.

**Note:** Use constraints and requirements from Phase 1.6 clarifications document.

1. **Design File Hierarchy**
   For each option, show:
   ```
   Option A: [Name]
   path/to/module.py
     class MainClass
       method_1()
     class HelperClass
   ```

2. **Create Dependency Graphs**
   Show how modules relate:
   ```
   MainClass
     └─> HelperClass
     └─> ExistingClass
   ```

3. **Propose 2-3 Options**
   For each:
   - Approach description
   - File structure
   - Dependencies
   - Trade-offs (pros/cons)

4. **Recommend an Option**
   Clear recommendation with rationale.

**Gate:** User selects or requests refinement.

---

### Phase 4: Task Decomposition

**Purpose:** Break PRD into parallelizable tasks with dependencies.

1. **Identify Task Boundaries**
   Look for tasks that:
   - Can be implemented independently
   - Have clear interfaces
   - Can be tested in isolation

2. **Define Tasks**
   For each task:
   ```markdown
   ### Task: task-name
   **Goal:** [One sentence]
   **Scope:** [What's included]
   **Complexity:** Simple | Moderate | Complex
   ```

3. **Analyze Dependencies**
   For each task:
   - **Depends On:** Which tasks must complete first
   - **Blocks:** Which tasks wait for this one

4. **Create Dependency Graph**
   ```
   Wave 1 (No dependencies):
   ├─ task-data-models
   └─ task-api-spec

   Wave 2 (Depends on Wave 1):
   ├─ task-business-logic
   │  └─ Depends on: task-data-models
   └─ task-ui-views
      └─ Depends on: task-api-spec

   Wave 3:
   └─ task-integration
      └─ Depends on: Wave 2
   ```

5. **Simplify Dependencies**
   - Break long chains
   - Define interfaces to decouple
   - Create foundation tasks

**Gate:** User approves task breakdown.

---

### Phase 5: System-Level OKRs

**Purpose:** Define technical success criteria (complement PRD's product OKRs).

1. **Define Technical Objective**
   One clear objective:
   "Deliver a scalable batch moderation system"

2. **Write System-Level Key Results**
   3-5 technical outcomes:
   - KR1: System processes 1000+ ops/sec with <100ms latency
   - KR2: All components have >80% test coverage
   - KR3: API handles 10x traffic spike
   - KR4: Code follows project conventions

   **Characteristics:**
   - System capabilities, not implementation
   - Performance/quality targets
   - Operational characteristics

3. **Map Tasks to OKRs**
   Show which tasks validate which KRs.

**Gate:** User approves system OKRs.

---

### Phase 6: Detailed Development Plan

**Purpose:** Create implementation roadmap for each task.

1. **Elaborate Each Task**
   ```markdown
   ### Task: task-name

   **Implementation Steps:**
   1. Define base classes
   2. Add validation
   3. Write tests

   **Acceptance Criteria:**
   - All tests pass
   - 80%+ coverage
   - Follows conventions

   **Effort:** Simple | Moderate | Complex
   ```

2. **Define Interfaces**
   Key function signatures and contracts.

3. **Identify Dependencies**
   External libraries, APIs, services needed.

4. **Create Test Strategy**
   - Unit tests
   - Integration tests
   - Manual testing

**Gate:** User reviews plan.

---

### Phase 7: Write Plan Document

**Purpose:** Save formal document.

Create: `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`

```markdown
# [Feature Name] Development Plan

**Date:** YYYY-MM-DD
**Status:** Approved
**PRD:** [link to PRD]
**Clarifications:** [link to YYYY-MM-DD-feature-name-clarifications.md]

## Overview
[Summary]

## Requirements
[Clarified from Phase 1 - reference clarifications document]

## System-Level OKRs
[From Phase 5]

## Risk Assessment
[From Phase 2]

## Design
[Selected option from Phase 3]

## Task Decomposition
[From Phase 4]

## Implementation Details
[From Phase 6]

## Test Strategy
[Testing approach]

## Acceptance Criteria
[Success criteria]
```

---

## Best Practices

### Question Resolution & Technical Interview
- Ask early, don't discover during implementation
- Be specific and systematic in generating questions
- Prioritize blocking vs. nice-to-clarify
- Request visual materials (architecture diagrams, data flows)
- Document all answers in clarifications document
- Confirm understanding by restating answers
- Consider alternative approaches even when requirements seem clear

### Design Options
- Show trade-offs for each
- Use existing patterns
- Consider maintainability

### Task Decomposition
- Maximize parallel work
- Keep dependencies simple
- Use interfaces to decouple
- Right-size tasks

### System OKRs
- Outcome-oriented, not implementation
- Measurable and verifiable
- Complement PRD's product OKRs
```

---

## Skill 5: feature-plan-review

**Purpose:** Review and validate development plan for completeness and feasibility.

**File:** `${TOOL_CONFIG}/skills/feature-plan-review/SKILL.md`

```markdown
---
name: feature-plan-review
description: Review and validate development plan for completeness and feasibility
disable-model-invocation: true
argument-hint: "[path-to-plan-file]"
---

# Feature Plan Review

Review a development plan for completeness, feasibility, and quality.

## Input

Path to plan file (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`)

## Output

Review report with:
- Validation results
- Identified gaps
- Actionable feedback
- Approval status

## Process

### Phase 1: Read Plan

Read the plan and extract all sections.

### Phase 2: Validate Task Decomposition

Check:
- [ ] Tasks are atomic and testable
- [ ] Dependencies clearly specified
- [ ] Dependency graph shows waves
- [ ] Independent tasks can parallelize
- [ ] No circular dependencies
- [ ] Each task has clear deliverables

### Phase 3: Validate Design Options

Check:
- [ ] At least 2 options presented
- [ ] Trade-offs documented
- [ ] Recommendation has rationale
- [ ] Aligns with codebase patterns

### Phase 4: Validate System OKRs

Check:
- [ ] Outcome-oriented (not implementation)
- [ ] Measurable
- [ ] All tasks map to OKRs
- [ ] Complements PRD OKRs

### Phase 5: Identify Gaps

Check for:
- Unresolved questions
- Missing risk mitigation
- Undefined interfaces
- Missing acceptance criteria

### Phase 6: Provide Feedback

```markdown
# Development Plan Review: [Feature Name]

**Reviewed:** YYYY-MM-DD
**Status:** Approved / Needs Changes

## Task Decomposition
- [x] Tasks are atomic
- [ ] Issue: Task 3 has circular dependency

## Design Validation
- [x] Multiple options
- [x] Trade-offs clear

## System OKRs
- [x] Outcome-oriented
- [ ] Issue: KR2 is implementation detail

## Approval Decision
- [ ] Approved
- [x] Needs Changes
```
```

---

## Skill 6: feature-impl

**Purpose:** Implement feature from development plan using TDD with progress tracking.

**File:** `${TOOL_CONFIG}/skills/feature-impl/SKILL.md`

```markdown
---
name: feature-impl
description: Implement feature from development plan using TDD with progress tracking
disable-model-invocation: true
argument-hint: "[path-to-plan-file]"
---

# Feature Implementation

Implement a feature from an approved development plan using Test-Driven Development.

## When to Use

Use when:
- Development plan is approved
- Ready to write code
- TDD workflow required

Do NOT use when:
- No plan exists (use feature-plan)
- Parallelizing tasks (use parallel-orchestrate)

## Input

- Path to development plan (e.g., `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`)

## Output

- Implemented feature with passing tests
- Updated documentation (CLAUDE.md, module READMEs)
- Code following project conventions

## Process

## Goal

Implement a feature from an approved development plan using TDD, following project conventions, with verified modularity and quality.

## OKRs

**Objective:** Deliver production-ready feature code that passes all quality gates

**Key Results:**
- KR1: All acceptance criteria from plan have passing tests (100% coverage of requirements)
- KR2: Test coverage ≥ [TEST_COVERAGE_THRESHOLD from Phase 1 repository interview]%
- KR3: All modularity criteria pass (see skill-guidelines/modularity.md)
- KR4: Code follows all conventions from CLAUDE.md (verified by linter/review)
- KR5: Performance requirements met (if applicable, from Phase 1 repository interview)
- KR6: Security requirements met (from Phase 1 repository interview)

## Evaluation Criteria

This skill is **complete** when ALL of the following are verified:

**Correctness:**
- [ ] All user stories from plan have acceptance tests: `pytest tests/acceptance/ -v`
- [ ] All acceptance tests PASS: [Number]/[Number] passing
- [ ] Edge cases tested and passing: [List specific edge cases from plan]
- [ ] Error conditions handled and tested: [List error scenarios]

**Quality:**
- [ ] Test coverage ≥ [THRESHOLD from Phase 1]%: `pytest --cov=src --cov-report=term`
- [ ] No linting errors: `pylint src/ --exit-zero` (score ≥ [THRESHOLD from Phase 1])
- [ ] Modularity assessment PASS: All 8 criteria ✅ (see Phase 7)
- [ ] Complexity within limits from Phase 1: `radon cc src/ -a -nb`

**Conventions:**
- [ ] Uses patterns from CLAUDE.md: [List specific patterns used]
- [ ] Naming matches project conventions: Verified in code review
- [ ] No code duplication: `jscpd src/` shows <3% duplication

**Completeness:**
- [ ] All tasks from plan implemented: [X]/[Y] tasks complete
- [ ] No TODOs or placeholders left: `grep -r "TODO\|FIXME\|XXX" src/` returns 0
- [ ] Documentation updated: CLAUDE.md and module READMEs current

**Security (if applicable):**
- [ ] Input validation for all user inputs
- [ ] No secrets in code: `git secrets --scan`
- [ ] Dependencies scanned: `safety check` (Python) or `npm audit` (JS)
- [ ] Security tools from Phase 1 pass with 0 high/medium issues
- [ ] Authentication/authorization requirements from Phase 1 met

**Performance (if applicable):**
- [ ] Response time < [THRESHOLD from Phase 1 repository interview]ms: `pytest tests/performance/`
- [ ] Memory usage acceptable: [specific threshold from Phase 1]
- [ ] No N+1 queries: Database query count logged and reviewed
- [ ] Database query performance meets Phase 1 requirements

**⚠️ Common Over-Evaluation Traps:**

**Trap: "Tests pass" ≠ Complete**
- ❌ "All tests pass" - Did you test ALL acceptance criteria? Edge cases? Error states?
- ✅ Map each acceptance criterion to specific test, verify all pass

**Trap: "Looks modular" ≠ Passes Modularity Assessment**
- ❌ "Code is well-organized" - Run actual tools: `radon cc`, `pylint`, `pydeps`
- ✅ All 8 modularity criteria evaluated with evidence (see Phase 7)

**Trap: "Follows conventions" ≠ Verified**
- ❌ "I followed the patterns" - Did you verify with grep/tools?
- ✅ List specific conventions from CLAUDE.md and show evidence of usage

**Trap: "No obvious bugs" ≠ Correct**
- ❌ "Works for me" - Did you test error states? Invalid inputs? Concurrent access?
- ✅ Acceptance tests cover happy path + edge cases + error states

### Phase 0: Read Documentation

**Purpose:** Understand project conventions before starting.

**READ REQUIRED FILES:**
1. Read CLAUDE.md (or equivalent project conventions file)
2. Read relevant module READMEs
3. Review coding standards
4. Note patterns to follow

**CONFIRM UNDERSTANDING:**
- What conventions apply to this feature?
- What existing patterns should I use?
- What tools/libraries are standard?
- What testing practices are required?

**PROGRESS TRACKING:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 FEATURE-IMPL PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
🔄 Phase 0: Read documentation [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 1: Research existing code
⏸️ Phase 2: Design structure
⏸️ Phase 3: Design scratchpad
⏸️ Phase 4: Write tests
⏸️ Phase 5: Implement feature
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 0: Reading CLAUDE.md and project conventions
Status: Understanding patterns and standards
Started: [TIME]

CHECKLIST:
✅ Read CLAUDE.md
🔲 Read module READMEs
🔲 Review coding standards
🔲 Confirm understanding
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Gate:** Conventions understood.

---

### Phase 1: Research Existing Code

**Purpose:** Find related patterns and understand codebase structure.

1. **Find Similar Features**
   - Search for similar implementations
   - Review existing patterns
   - Note code organization

2. **Identify Dependencies**
   - What modules will this use?
   - What interfaces exist?
   - What utilities are available?

3. **Check for Reuse**
   - Can existing code be reused?
   - Should this extend existing classes?
   - Are there shared utilities?

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
🔄 Phase 1: Research existing code [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Design structure
⏸️ Phase 3: Design scratchpad
⏸️ Phase 4: Write tests
⏸️ Phase 5: Implement feature
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 1: Researching similar features and patterns
Status: Found 3 similar implementations to review
Started: [TIME]

CHECKLIST:
✅ Search for similar features
🔲 Review existing patterns
🔲 Identify dependencies
🔲 Check for reuse opportunities
```

**Gate:** Related code reviewed.

---

### Phase 2: Design Structure

**Purpose:** Plan file/module organization before writing code.

**WITHOUT WRITING CODE, design:**

1. **File Structure**
   ```
   module/
   ├── __init__.py
   ├── feature.py       # Main implementation
   ├── helpers.py       # Supporting functions
   └── tests/
       ├── test_feature.py
       └── test_helpers.py
   ```

2. **Module Dependencies**
   Show import structure:
   ```python
   # feature.py will import:
   from module.helpers import ...
   from existing.module import ...
   ```

3. **Key Functions/Classes**
   Function signatures only:
   ```python
   def main_function(arg1: Type1, arg2: Type2) -> ReturnType:
       """What it does."""
       pass
   ```

4. **Test Strategy**
   - Unit tests for each function
   - Integration tests for workflows
   - Edge cases to cover

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
🔄 Phase 2: Design structure [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: Design scratchpad
⏸️ Phase 4: Write tests
⏸️ Phase 5: Implement feature
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 2: Designing file structure and module organization
Status: Defining function signatures and dependencies
Started: [TIME]

CHECKLIST:
✅ Define file structure
✅ Map module dependencies
🔲 Design function signatures
🔲 Plan test strategy
```

**Gate:** User approves structure.

---

### Phase 3: Design Scratchpad

**Purpose:** Think through implementation to catch problems BEFORE writing code.

**Create:** `${SPEC_DIR}/YYYY-MM-DD-feature-name-scratchpad.md`

**Template:**

````markdown
# Implementation Scratchpad: [Feature Name]

**Date:** YYYY-MM-DD
**Plan:** ${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md

## Initial Approach

**Files to create/modify:**
- path/to/file1.py - [What it does]
- path/to/file2.py - [What it does]
- tests/test_file1.py - [Test coverage]

**Key functions/classes:**
```python
def main_function(arg1: Type1, arg2: Type2) -> ReturnType:
    """What this does."""
    # Pseudocode implementation idea
    pass

class MainClass:
    """What this handles."""
    def method1(self):
        pass
```

**Dependencies:**
- Existing module X (for Y functionality)
- Library Z (for handling W)

## Problems I Notice

### Problem 1: [Describe issue]
**Why it's a problem:** [Explain]
**Fix:** [How to address]

### Problem 2: [Describe issue]
**Why it's a problem:** [Explain]
**Fix:** [How to address]

## Assessment Against Criteria

**Modularity (Design Phase):**
- [ ] Cohesion: Each module has single purpose? [Y/N + evidence]
- [ ] Coupling: Dependencies minimal (<5)? [Y/N + list dependencies]
- [ ] Complexity: Functions will be <15 complexity? [Y/N + estimate]
- [ ] Testability: Can mock dependencies easily? [Y/N + explain]

**Completeness:**
Map to acceptance criteria from plan:
- [ ] Acceptance criterion 1: [How implementation covers this]
- [ ] Acceptance criterion 2: [How implementation covers this]
- [ ] Edge case A: [How handled]
- [ ] Error condition B: [How handled]

**Conventions:**
- [ ] Uses pattern X from CLAUDE.md? [Y/N + where]
- [ ] Follows naming convention Y? [Y/N + examples]
- [ ] No duplication of existing code? [Y/N + checked where]

**Security (if applicable):**
- [ ] Input validation needed? [Where + how]
- [ ] Authentication required? [How handled]
- [ ] Sensitive data? [How protected]

**Performance (if applicable):**
- [ ] Will meet <[X]ms requirement? [Y/N + why]
- [ ] Database queries optimized? [How]
- [ ] Caching needed? [Where]

## Revised Approach (if changes needed)

[If problems found above, revise design here]

**Files to create/modify (revised):**
- [Updated list]

**Key functions/classes (revised):**
```python
[Updated design]
```

**Why this is better:**
- [Explain improvements]

## Self-Review Checklist

- [ ] Read similar code in codebase for patterns
- [ ] Drew dependency diagram (no circular deps)
- [ ] Can test each component in isolation
- [ ] All acceptance criteria mapped to code
- [ ] No obvious security/performance issues
- [ ] Follows CLAUDE.md conventions

## Sign-Off

Ready to proceed: ✅ / ❌
If ❌, what needs more thought: [Explain]
````

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
🔄 Phase 3: Design scratchpad [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Write tests
⏸️ Phase 5: Implement feature
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 3: Creating design scratchpad to catch issues early
Status: Reviewing design for modularity and completeness
Started: [TIME]

CHECKLIST:
✅ Document initial approach
✅ Identify potential problems
🔲 Assess against modularity criteria
🔲 Map to acceptance criteria
🔲 Check conventions
🔲 Self-review complete
```

**Gate:** Scratchpad review complete, no blocking issues identified

---

### Phase 4: Write Tests (TDD)

**Purpose:** Write tests BEFORE implementation.

1. **Write Test Cases**
   For each function/class:
   - Happy path tests
   - Edge case tests
   - Error handling tests

2. **Verify Tests Fail**
   Run tests - they should fail (implementation doesn't exist yet).

3. **Review Test Quality**
   - Clear test names
   - Independent tests
   - Good assertions

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
✅ Phase 3: Design scratchpad
🔄 Phase 4: Write tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 5: Implement feature
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 4: Writing tests before implementation (TDD)
Status: Writing tests for main_function
Started: [TIME]

CHECKLIST:
✅ Write happy path tests
🔲 Write edge case tests
🔲 Write error handling tests
🔲 Verify tests fail
```

**Gate:** Tests written and failing.

---

### Phase 5: Implement Feature

**Purpose:** Write code to make tests pass.

1. **Implement Functions**
   - Start with simplest function
   - Make tests pass one at a time
   - Refactor as needed

2. **Follow Conventions**
   - Use patterns from Phase 0
   - Follow code style from CLAUDE.md
   - Match existing structure

3. **Add Documentation**
   - Docstrings for all functions/classes
   - Inline comments for complex logic
   - Type hints

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
✅ Phase 3: Design scratchpad
✅ Phase 4: Write tests
🔄 Phase 5: Implement feature [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 6: Run tests
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 5: Implementing feature code
Status: Writing main_function implementation
Started: [TIME]

CHECKLIST:
✅ Implement helper functions
🔲 Implement main function
🔲 Add docstrings
🔲 Add type hints
```

**Gate:** Implementation complete.

---

### Phase 6: Run Tests

**Purpose:** Verify implementation works.

1. **Run Test Suite**
   ```bash
   pytest path/to/tests/
   ```

2. **Fix Failures**
   If tests fail:
   - Debug the issue
   - Fix implementation
   - Re-run tests

3. **Check Coverage**
   - Aim for >80% coverage
   - Add tests for uncovered branches

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
✅ Phase 3: Design scratchpad
✅ Phase 4: Write tests
✅ Phase 5: Implement feature
🔄 Phase 6: Run tests [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 7: Assess quality
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 6: Running tests and verifying implementation
Status: All tests passing, checking coverage
Started: [TIME]

CHECKLIST:
✅ Run test suite
✅ Fix test failures
🔲 Check coverage
🔲 Add missing tests
```

**Gate:** All tests pass, coverage >80%.

---

### Phase 7: Assess Quality

**Purpose:** Comprehensive quality assessment before marking complete.

**Note:** Use evaluation criteria from Phase 1 (repository-specific interview) as the baseline for all assessments.

**Assessment Types:**

#### 7.1: Modularity Assessment

**Reference:** skill-guidelines/modularity.md

**Run comprehensive assessment:**

```bash
# Create assessment document
cp skill-guidelines/modularity-assessment-template.md \
   ${SPEC_DIR}/YYYY-MM-DD-feature-name-modularity-assessment.md

# Run automated tools
pylint --enable=R0904,R0902 src/new_module.py
radon cc src/new_module.py -a -nb
pydeps src/new_module.py --show-deps
pytest tests/unit/test_new_module.py --verbose
```

**Fill out assessment template with evidence.**

**Gate:** All 8 modularity criteria ✅ or ⚠️ (no ❌)

#### 7.2: Correctness Assessment

**Verify completeness:**

```bash
# Map acceptance criteria to tests
grep -r "test_.*acceptance" tests/

# Check edge cases covered
grep -r "test_.*edge" tests/

# Check error handling
grep -r "test_.*error\|test_.*invalid" tests/
```

**Manual verification:**
- [ ] Every acceptance criterion has a test
- [ ] Every edge case from plan has a test
- [ ] Every error condition has a test
- [ ] All tests PASS

**Gate:** 100% acceptance criteria coverage, all tests pass

#### 7.3: Convention Assessment

**Check against CLAUDE.md:**

```bash
# Naming conventions
grep -r "def \|class " src/new_module.py
# Compare against patterns in CLAUDE.md

# No duplication
jscpd src/new_module.py --min-tokens 50

# Linting
pylint src/new_module.py --exit-zero
```

**Manual verification:**
- [ ] Uses patterns from CLAUDE.md: [List specific ones]
- [ ] Naming follows conventions: [Examples]
- [ ] No duplicated code: jscpd shows <3%
- [ ] Linter score ≥ [THRESHOLD from Phase 1]
- [ ] Complexity within limits from Phase 1

**Gate:** All conventions followed

#### 7.4: Security Assessment (if applicable)

**Run security checks:**

```bash
# Python
bandit -r src/new_module.py
safety check

# JavaScript
npm audit
eslint --plugin security src/

# Check for secrets
git secrets --scan
```

**Manual verification:**
- [ ] All user inputs validated
- [ ] No secrets in code
- [ ] Dependencies have no known vulnerabilities
- [ ] Authentication/authorization correct
- [ ] Security tools from Phase 1 all pass
- [ ] Secret handling follows Phase 1 requirements

**Gate:** No security issues found, all Phase 1 security requirements met

#### 7.5: Performance Assessment (if applicable)

**Run performance tests:**

```bash
pytest tests/performance/test_feature_performance.py -v
```

**Verify requirements:**
- [ ] Response time < [THRESHOLD from Phase 1]ms: [Actual time]
- [ ] Memory usage acceptable: [Actual usage vs Phase 1 limit]
- [ ] Database queries optimized: [Query count vs Phase 1 requirements]
- [ ] All Phase 1 performance benchmarks met

**Gate:** All performance requirements from Phase 1 met

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
✅ Phase 3: Design scratchpad
✅ Phase 4: Write tests
✅ Phase 5: Implement feature
✅ Phase 6: Run tests
🔄 Phase 7: Assess quality [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 8: Update documentation

CURRENT TASK:
Phase 7: Running comprehensive quality assessment
Status: Evaluating modularity, correctness, conventions
Started: [TIME]

ASSESSMENT RESULTS:
✅ Modularity: 8/8 criteria pass
✅ Correctness: 15/15 acceptance tests pass
✅ Conventions: Linter score 9.2/10
✅ Security: 0 vulnerabilities found
✅ Performance: 95th percentile 87ms (< 100ms threshold)
```

**Gate:** All quality assessments pass

---

### Phase 8: Update Documentation (Final Phase)

**Purpose:** Maintain project conventions and prevent documentation drift.

**UPDATE REQUIRED FILES:**

1. **Update CLAUDE.md**
   - Add new patterns introduced
   - Document new conventions
   - Add usage examples

2. **Update Module READMEs**
   - Document new functions/classes
   - Add usage examples
   - Note dependencies

3. **Check for Drift**
   - Did implementation follow conventions?
   - Were existing patterns used?
   - Any new patterns that should be documented?

4. **Document Assessment Results**
   - Reference modularity assessment from Phase 7
   - Note any conventions verified
   - Record performance benchmarks (if applicable)

**PROGRESS TRACKING:**
```
PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: Research existing code
✅ Phase 2: Design structure
✅ Phase 3: Design scratchpad
✅ Phase 4: Write tests
✅ Phase 5: Implement feature
✅ Phase 6: Run tests
✅ Phase 7: Assess quality
🔄 Phase 8: Update documentation [IN PROGRESS] ◀── YOU ARE HERE

CURRENT TASK:
Phase 8: Updating documentation to prevent drift
Status: Updating CLAUDE.md with new patterns
Started: [TIME]

CHECKLIST:
✅ Update CLAUDE.md
🔲 Update module READMEs
🔲 Check for convention drift
🔲 Document assessment results
🔲 Final review
```

**Output:**
- Feature complete
- Tests passing
- Quality assessments passed
- Documentation current

---

## Best Practices

### TDD Workflow
- Always write tests first (Phase 4)
- Verify tests fail before implementing
- Make tests pass incrementally (Phase 5)
- Refactor with confidence

### Design Before Implementation
- Use Phase 3 scratchpad to think through design
- Catch modularity issues early
- Map acceptance criteria to implementation
- Identify problems before writing code

### Documentation Maintenance
- Read conventions in Phase 0
- Follow patterns during implementation
- Update docs in Phase 8 (Final Phase)
- Prevents drift over time

### Quality Assessment
- Run comprehensive assessment in Phase 7
- Use automated tools for modularity, security, performance
- Verify all acceptance criteria covered
- Don't skip quality gates

### Progress Tracking
- Update status after each phase
- Report current task and checklist
- Creates audit trail
- Less likely to skip steps
```

---

## Phase 3: Integration

### Complete Workflow Integration

The feature development skills form a cohesive workflow with clear handoffs:

```
┌─────────────────────────────────────────────────────────────┐
│                    feature-workflow                          │
│                    (Orchestrator)                            │
└─────────────────────────────────────────────────────────────┘
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
┌─────────┐         ┌──────────┐         ┌──────────┐
│ Phase 0 │         │ Phase 1  │         │ Phase 2  │
│ Detect  │────────▶│ Repo     │────────▶│ Define   │
│ State   │         │Interview │         │ (PRD)    │
└─────────┘         └──────────┘         └──────────┘
                          │                     │
                          │                     ▼
                          │            ┌────────────────┐
                          │            │ feature-define │
                          │            │    (Atomic)    │
                          │            └────────────────┘
                          │                     │
                          │                     ▼
                          │            ┌────────────────┐
                          │            │feature-define- │
                          │            │   review       │
                          │            │  (Optional)    │
                          │            └────────────────┘
                          │                     │
                          │                     ▼
                          │            ┌──────────┐
                          └────────────│ Phase 4  │
                                       │ Plan     │
                                       │ (Plan)   │
                                       └──────────┘
                                            │
                                            ▼
                                   ┌────────────────┐
                                   │ feature-plan   │
                                   │   (Atomic)     │
                                   └────────────────┘
                                            │
                                            ▼
                                   ┌────────────────┐
                                   │feature-plan-   │
                                   │  review        │
                                   │  (Optional)    │
                                   └────────────────┘
                                            │
                    ┌───────────────────────┼────────────────┐
                    │                       │                │
                    ▼                       ▼                ▼
            ┌──────────────┐       ┌──────────────┐  ┌──────────┐
            │ Phase 6      │       │ Phase 9      │  │ Phase 10 │
            │ Decide       │──────▶│ Implement    │─▶│ Review   │
            │ Execution    │       │              │  │ Quality  │
            └──────────────┘       └──────────────┘  └──────────┘
                    │                     │
                    │                     ▼
                    │            ┌────────────────┐
                    │            │ feature-impl   │
                    │            │   (Atomic)     │
                    │            └────────────────┘
                    │
                    ▼
         ┌─────────────────────┐
         │ parallel-orchestrate│
         │  (If parallelizable)│
         └─────────────────────┘
```

### State Detection Flow

The orchestrator detects existing state and resumes from the appropriate phase:

```
User invokes: /feature-workflow

                    ┌─────────────┐
                    │   Detect    │
                    │   State     │
                    └──────┬──────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
    ┌─────────┐      ┌─────────┐     ┌─────────┐
    │PRD      │      │Plan     │     │Code     │
    │exists?  │      │exists?  │     │exists?  │
    └────┬────┘      └────┬────┘     └────┬────┘
         │                │               │
    YES  │  NO       YES  │  NO      YES  │  NO
         │                │               │
    ┌────▼───┐       ┌────▼───┐      ┌────▼───┐
    │Skip to │       │Skip to │      │Resume  │
    │Phase 4 │       │Phase 6 │      │from    │
    │(Plan)  │       │(Decide)│      │last    │
    └────────┘       └────────┘      │task    │
                                     └────────┘
         │                │               │
         └────────────────┴───────────────┘
                          │
                          ▼
                   ┌──────────────┐
                   │   Continue   │
                   │   Workflow   │
                   └──────────────┘
```

### Skill Composition Patterns

**Sequential Composition** (feature-workflow calls skills in order):
```
feature-workflow
  └─► Phase 0: Detect state
        └─► Phase 1: Repository interview
              └─► Produces: ${SPEC_DIR}/repository-evaluation-criteria.md
                    └─► Phase 2: feature-define
                          └─► Produces: ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md
                                └─► Phase 4: feature-plan
                                      └─► Produces: ${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md
                                            └─► Phase 9: feature-impl
                                                  └─► Produces: Working code + tests
                                                        └─► Phase 10: Review modularity
                                                              └─► Verify against Phase 1 criteria
```

**Review Integration** (optional structured feedback):
```
feature-define
  └─► PRD created
        └─► User review gate (simple approval)
              └─► OR: /feature-define-review (structured feedback)
                    └─► Detailed checklist + recommendations
```

**Parallel Branching** (orchestrator decides):
```
feature-plan
  └─► Plan with task dependency graph
        └─► feature-workflow Phase 6 analyzes
              │
              ├─► Independent tasks? → /parallel-orchestrate
              │
              └─► Sequential tasks? → Phase 9: /feature-impl
```

### Data Flow

**Artifacts produced at each stage:**

| Phase | Skill | Input | Output |
|-------|-------|-------|--------|
| 0 | State detection | - | Starting phase |
| 1 | Repository interview | User responses | `repository-evaluation-criteria.md` |
| 2 | feature-define | Feature idea | `*-prd.md` |
| 3 | feature-define-review | `*-prd.md` | Review report |
| 4 | feature-plan | `*-prd.md` | `*-plan.md` |
| 5 | feature-plan-review | `*-plan.md` | Review report |
| 6 | Execution decision | `*-plan.md` | Strategy (parallel/sequential) |
| 7-8 | Implementation proposal | `*-plan.md` | Proposal + review |
| 9 | feature-impl | `*-plan.md` | Code + tests + docs |
| 10 | Modularity review | Code + Phase 1 criteria | Quality assessment |

**File naming conventions:**
- PRDs: `${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md`
- Plans: `${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md`
- Reviews: Console output or comments in original file

---

## Phase 4: Testing & Validation

### Validation Strategy

Test each skill individually, then test the complete workflow.

### Test 1: feature-define

**Scenario:** Create PRD for a simple feature

**Steps:**
1. Invoke: `/feature-define "Add dark mode toggle to settings"`
2. Verify all phases execute
3. Check PRD has all required sections
4. Validate user stories follow format
5. Validate OKRs are outcome-oriented

**Expected Output:**
- `${SPEC_DIR}/YYYY-MM-DD-dark-mode-prd.md`
- Contains: Background, User Stories, OKRs, UI Design, Risks, Scope
- User stories use "As a [persona], I want [capability], so that [benefit]"
- OKRs describe outcomes, not implementation

### Test 2: feature-plan

**Scenario:** Create development plan from PRD

**Steps:**
1. Use PRD from Test 1
2. Invoke: `/feature-plan ${SPEC_DIR}/YYYY-MM-DD-dark-mode-prd.md`
3. Verify context gathering (reads CLAUDE.md)
4. Check design options presented (2-3 options)
5. Validate task decomposition shows dependencies
6. Verify system OKRs are technical (not product)

**Expected Output:**
- `${SPEC_DIR}/YYYY-MM-DD-dark-mode-plan.md`
- Contains: Requirements, Risk Assessment, Design, Tasks, System OKRs
- Task dependency graph shows waves
- System OKRs focus on performance/quality

### Test 3: feature-impl

**Scenario:** Implement feature from plan

**Steps:**
1. Use plan from Test 2
2. Invoke: `/feature-impl ${SPEC_DIR}/YYYY-MM-DD-dark-mode-plan.md`
3. Verify Phase 0 reads CLAUDE.md
4. Verify Phase 3 creates design scratchpad
5. Check progress tracking displays at each phase (0-8)
6. Verify tests written before implementation (Phase 4 - TDD)
7. Verify Phase 7 runs quality assessment
8. Verify Phase 8 updates documentation

**Expected Output:**
- Working code matching plan
- All tests pass
- Coverage >80%
- Design scratchpad created with assessment
- Quality assessment passed (modularity, correctness, conventions)
- CLAUDE.md updated if new patterns introduced
- Progress tracker showed all 9 phases (0-8)

### Test 4: Review Skills

**Scenario:** Review PRD with issues

**Steps:**
1. Create PRD with intentional issues (vague user stories, implementation-focused OKRs)
2. Invoke: `/feature-define-review ${SPEC_DIR}/YYYY-MM-DD-test-prd.md`
3. Verify issues identified
4. Check feedback is actionable

**Expected Output:**
- Review report with specific issues
- Recommendations for improvement
- Clear approval decision

### Test 5: Complete Workflow

**Scenario:** End-to-end feature development

**Steps:**
1. Invoke: `/feature-workflow`
2. Provide feature idea when prompted
3. Approve PRD at review gate
4. Approve plan at review gate
5. Approve implementation proposal
6. Verify implementation completes

**Expected Output:**
- PRD created in ${SPEC_DIR}
- Plan created in ${SPEC_DIR}
- Working code with tests
- All review gates honored
- Modularity assessment provided

### Test 6: State Detection

**Scenario:** Resume workflow from existing PRD

**Steps:**
1. Create PRD manually or via `/feature-define`
2. Invoke: `/feature-workflow`
3. Verify it detects existing PRD
4. Verify it skips to Phase 4 (Plan Implementation)

**Expected Output:**
- Workflow detects PRD exists
- Skips Phase 2 (Define Requirements)
- May still run Phase 1 (Repository Interview) if criteria not documented
- Starts from Phase 4 (Plan Implementation)

### Test 7: Parallel vs Sequential

**Scenario:** Workflow decides execution strategy

**Steps:**
1. Create plan with independent tasks
2. Run `/feature-workflow` with that plan
3. Verify it suggests parallel execution

**Expected Output:**
- Workflow analyzes task dependencies
- Identifies parallelization opportunity
- Suggests `/parallel-orchestrate`

---

## Phase 5: Customization

### Adapting to Your Project

The feature development suite uses placeholders that should be customized:

**Environment Variables:**
- `${SPEC_DIR}` - Where PRDs and plans are stored
- `${TOOL_CONFIG}` - AI tool's configuration directory

**Project-Specific Elements:**
- Document naming pattern (YYYY-MM-DD-feature-name or other)
- Approval process (who reviews PRDs/plans?)
- Testing framework (pytest, jest, cargo test, etc.)
- Code conventions (from CLAUDE.md or CONTRIBUTING.md)

### Customization Checklist

```markdown
## Feature Suite Customization

- [ ] Set ${SPEC_DIR} to your specs location
- [ ] Set ${TOOL_CONFIG} to your AI tool's config directory
- [ ] Update document naming pattern in templates
- [ ] Customize review gates (frequency, who approves)
- [ ] Update test commands for your framework
- [ ] Add project-specific risk categories
- [ ] Customize modularity criteria
- [ ] Update progress tracking format if desired
```

### Common Customizations

**1. Review Gate Frequency**

Adjust how often user approval is required:
- **Minimal:** Only approve final implementation
- **Standard:** Approve PRD, plan, implementation proposal
- **Thorough:** Approve each section of PRD and plan

**2. Document Format**

Adapt PRD/plan templates to your organization:
- Add sections for compliance, security, etc.
- Change section order
- Add custom metadata (team, priority, etc.)

**3. Task Decomposition Strategy**

Customize how tasks are broken down:
- Prefer smaller vs. larger tasks
- Different dependency visualization
- Custom complexity ratings

**4. Progress Tracking Style**

Modify the progress tracker appearance:
- Simpler text format
- More/less verbose
- Different symbols or formatting

---

## Phase 6: Best Practices

### Progress Tracking

**Why it matters:**
- Makes Claude report what it's doing (less likely to skip steps)
- Provides audit trail
- User can see current state

**How to use:**
- ALL workflow skills (feature-workflow, feature-impl) include progress tracking
- Progress updates after each phase completion
- Shows current task and checklist items

**Example from feature-impl:**
```
PHASES:
✅ Phase 0: Read documentation
🔄 Phase 1: Research existing code [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 2: Design structure

CURRENT TASK:
Phase 1: Researching similar features and patterns
Status: Found 3 similar implementations to review
Started: [TIME]

CHECKLIST:
✅ Search for similar features
🔲 Review existing patterns
```

### Documentation Maintenance

**Why it matters:**
- Prevents convention drift
- Prevents code duplication
- Ensures patterns are followed

**How to use:**
- Phase 0: Read CLAUDE.md and project conventions
- Final Phase: Update CLAUDE.md if new patterns introduced
- Check for drift: Did implementation follow conventions?

**Required in:**
- `feature-impl` (reads Phase 0, updates Final Phase)
- Any implementation skill

### Review Gates

**Purpose:**
- User alignment before proceeding
- Catch issues early
- Prevent wasted implementation effort

**When to use:**
- After PRD creation (ensure requirements correct)
- After plan creation (validate technical approach)
- Before implementation (approve design)

**Optional, not mandatory:**
- Users can skip gates if they trust the output
- Review skills provide structured feedback when needed

### State Detection

**Purpose:**
- Resume from any point in workflow
- Don't duplicate work
- Support iterative development

**How it works:**
- Check for existing PRD → skip to planning
- Check for existing plan → skip to implementation
- Check for partial code → resume from last task

**Implemented in:**
- `feature-workflow` Phase 0

### TDD Workflow

**Purpose:**
- Catch bugs early
- Design better interfaces
- Refactor with confidence

**Process:**
1. Write tests first (Phase 3)
2. Verify tests fail
3. Implement to pass tests (Phase 4)
4. Verify tests pass (Phase 5)
5. Refactor with confidence

**Required in:**
- `feature-impl`

---

## Summary

### Skills Created

After following this guide, you will have:

- `${TOOL_CONFIG}/skills/feature-workflow/SKILL.md` - Orchestrator
- `${TOOL_CONFIG}/skills/feature-define/SKILL.md` - PRD creation
- `${TOOL_CONFIG}/skills/feature-define-review/SKILL.md` - PRD validation
- `${TOOL_CONFIG}/skills/feature-plan/SKILL.md` - Development planning
- `${TOOL_CONFIG}/skills/feature-plan-review/SKILL.md` - Plan validation
- `${TOOL_CONFIG}/skills/feature-impl/SKILL.md` - TDD implementation

### Key Capabilities

**Requirements Definition:**
- Lightweight PRDs with user stories and OKRs
- Background, UI design, risks, scope boundaries
- Review validation for completeness

**Development Planning:**
- Question resolution before design
- Risk assessment and mitigation
- Multiple design options with trade-offs
- Task decomposition with dependency graphs
- System-level OKRs for technical success

**Implementation:**
- TDD workflow (tests before code)
- Documentation maintenance (read Phase 0, update Final Phase)
- Progress tracking throughout
- Convention following

**Orchestration:**
- State detection (resume from any point)
- Review gates (user alignment)
- Parallel vs. sequential decision
- Modularity evaluation

### Workflow Patterns

**Complete Feature Workflow:**
```
Feature Idea
  → feature-define → PRD
    → Review gate
      → feature-plan → Development plan
        → Review gate
          → feature-impl → Working code + tests
            → Modularity review
              → Complete feature
```

**State-Based Resume:**
```
PRD exists → Skip to planning
Plan exists → Skip to implementation
Code exists → Resume from last task
```

**Parallel Execution:**
```
Independent tasks detected
  → Switch to parallel-orchestrate
    → Multiple sessions work in parallel
      → Integration when complete
```

### Next Steps

1. **Create skills** using templates in this guide
2. **Test individually** using validation scenarios
3. **Test complete workflow** end-to-end
4. **Customize** for your project conventions
5. **Document** any project-specific adaptations

### Related Guides

- **Parallel Orchestration:** For coordinating parallel task execution
- **Refactoring:** For improving existing code quality
- **Bugfixing:** For hypothesis-based debugging
- **Code Review:** For automated review feedback

---

## Appendix: Quick Reference

### Invocation Commands

```bash
# Orchestrator (detects state, runs complete workflow)
/feature-workflow

# Individual skills
/feature-define "Feature description"
/feature-define path/to/feature-idea.md

/feature-plan ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md

/feature-impl ${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md

# Review skills
/feature-define-review ${SPEC_DIR}/YYYY-MM-DD-feature-name-prd.md
/feature-plan-review ${SPEC_DIR}/YYYY-MM-DD-feature-name-plan.md
```

### File Locations

```
${SPEC_DIR}/
├── YYYY-MM-DD-feature-name-prd.md    # PRDs
├── YYYY-MM-DD-feature-name-plan.md   # Plans
└── ...

${TOOL_CONFIG}/skills/
├── feature-workflow/SKILL.md
├── feature-define/SKILL.md
├── feature-define-review/SKILL.md
├── feature-plan/SKILL.md
├── feature-plan-review/SKILL.md
└── feature-impl/SKILL.md
```

### Decision Tree

```
Do you have a feature idea?
├─ YES → Start with /feature-workflow or /feature-define
└─ NO → Define the problem first

Do you have a PRD?
├─ YES → Use /feature-plan or /feature-workflow
└─ NO → Use /feature-define

Do you have a development plan?
├─ YES → Use /feature-impl or /feature-workflow
└─ NO → Use /feature-plan

Do tasks have dependencies?
├─ YES → Use /feature-impl (sequential)
└─ NO → Consider /parallel-orchestrate

Do you need structured feedback?
├─ YES → Use review skills
└─ NO → Simple approval at gates is fine
```
