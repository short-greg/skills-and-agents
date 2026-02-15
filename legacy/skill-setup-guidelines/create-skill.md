# Create Custom Skill Guide

Guide for creating custom skills that follow the skills-and-agents framework conventions.

Use this when you want to create a custom skill or workflow for your repository - whether a single atomic skill or a multi-skill workflow.

---

## When to Use

**Use this guide when:**
- You want a skill for a workflow not covered by existing skills
- You need a domain-specific skill for your repository
- You want to create a workflow that orchestrates multiple skills

**Examples:**
- "I want a skill for creating API endpoints"
- "I need a database migration workflow"
- "I want to create a deployment skill"
- "I need a skill for generating documentation"

---

## Prerequisites

- Repository already set up with framework (see [setup-repo.md](setup-repo.md))
- You understand your repo type (prototype/production/library)
- You have read [SETUP.md](../SETUP.md)
- You have read the framework references:
  - [references/goals-and-objectives.md](../references/goals-and-objectives.md)
  - [references/checklist-management.md](../references/checklist-management.md)
  - [references/assessment-approaches.md](../references/assessment-approaches.md)
  - [references/spec-maintenance.md](../references/spec-maintenance.md)

---

## Workflow

### Phase 0: Read Framework Documentation

**Purpose:** Understand framework conventions before creating a skill.

**Required reading:**

[See [SETUP.md](../SETUP.md) for core conventions]

- **[SETUP.md](../SETUP.md)** - Overall framework structure and conventions
- **[references/goals-and-objectives.md](../references/goals-and-objectives.md)** - How to write measurable OKRs
- **[references/checklist-management.md](../references/checklist-management.md)** - Checklist format and patterns
- **[references/assessment-approaches.md](../references/assessment-approaches.md)** - Quality assessment criteria
- **[references/spec-maintenance.md](../references/spec-maintenance.md)** - How to document progress

**Key concepts to understand:**
- Skills must have Goal + OKRs (outcome-oriented, measurable)
- Checklists use numbered items with conditions in parentheses
- Implementation skills must have Phase 0 (Read Documentation) and Final Phase (Update Documentation)
- Every checklist must include progress update item
- Customize for repo type (prototype/production/library)

**Checklist:**
- [ ] 1. Read SETUP.md core conventions
- [ ] 2. Read all references/ documents
- [ ] 3. Review existing skill examples from skill-setup-guidelines/
- [ ] 4. Understand framework patterns

**Gate:** Framework conventions understood.

---

### Phase 1: Define Skill Purpose

**Purpose:** Understand what skill(s) to create through interview process.

**Interview questions:**

#### 1. What problem does this solve?

Ask yourself:
- What workflow is currently manual that should be systematic?
- What quality gates are missing?
- What documentation process needs to be formalized?
- What repetitive task could benefit from a structured approach?

**Example answers:**
- "We create API endpoints manually without consistent structure"
- "Database migrations are ad-hoc, sometimes we forget to test rollback"
- "Documentation is scattered, no single process for creating examples"

#### 2. Single skill or multi-skill workflow?

**Single atomic skill:**
- Does one specific task
- Can be used standalone
- No dependencies on other skills
- Example: create-api-endpoint, write-migration-script

**Multi-skill workflow:**
- Requires multiple distinct phases
- Some phases are planning, some are implementation
- Orchestrator coordinates the workflow
- Example: Database migration (plan → execute), Feature development (define → plan → implement)

**Decision criteria:**
- If task can be done in one session with one spec/output → Single skill
- If task requires multiple specs (PRD + plan) or distinct review gates → Multi-skill workflow

#### 3. If workflow, what skills are needed?

**For multi-skill workflows, identify:**

**Planning skills:**
- Create specifications, PRDs, designs
- Output: Documentation to guide implementation
- No code changes

**Implementation skills:**
- Write code based on specs
- Run tests, quality checks
- Output: Working code

**Review skills:**
- Validate work meets requirements
- Run comprehensive assessments
- Output: Quality report

**Orchestrator skill:**
- Calls other skills in sequence
- Manages handoffs between phases
- Includes user review gates
- Output: Completed workflow

**Common patterns:**

**2-skill workflow:**
- [task]-impl: Implementation
- [task]-workflow: Orchestrates impl + any setup

**3-skill workflow:**
- [task]-plan: Create plan/spec
- [task]-impl: Implement from plan
- [task]-workflow: Orchestrates both

**4-skill workflow:**
- [task]-define: Create high-level spec
- [task]-plan: Create technical plan
- [task]-impl: Implement
- [task]-workflow: Orchestrates all three

#### 4. Document your decision

**Example 1 (single skill):**
```markdown
## Skill Definition

**Problem:** API endpoint creation is inconsistent
**Solution:** Single skill "create-api-endpoint"
**Type:** Implementation skill
**Input:** Endpoint description
**Output:** API endpoint code + tests
```

**Example 2 (multi-skill workflow):**
```markdown
## Skill Definition

**Problem:** Database migrations are ad-hoc and risky
**Solution:** Multi-skill workflow with 3 skills
**Skills:**
1. migration-plan (Planning) - Design migration, test plan
2. migration-execute (Implementation) - Apply migration with safety checks
3. migration-workflow (Orchestrator) - Coordinates plan → review → execute
```

**Gate:** Clear understanding of what skills to create.

---

### Phase 2: Design Skill Structure

**Purpose:** Plan phases, checklists, and quality gates for each skill.

**For each skill in your workflow, complete this design:**

#### Step 1: Determine Skill Type

**Planning skill:**
- Creates specs, PRDs, designs
- No code implementation
- Output: Documentation
- No Phase 0 or Final Phase needed

**Implementation skill:**
- Writes/modifies code
- Must include Phase 0: Read Documentation
- Must include Final Phase: Update Documentation
- Quality gates throughout

**Review skill:**
- Validates existing work
- Runs assessments
- No code changes
- Output: Assessment report

**Orchestrator skill:**
- Calls other skills
- Manages workflow
- User review gates
- No direct implementation

#### Step 2: List Phases

**Every skill needs:**
- Clear phases with specific purposes
- Each phase has a gate (what must pass to proceed)
- Logical progression

**Implementation skills must include:**
- Phase 0: Read Documentation (first)
- Phase N: Final documentation update (last)

**Common phase structures:**

**Planning skill phases:**
```markdown
1. Gather requirements
2. Design/architect
3. Write specification
4. Review with user
```

**Implementation skill phases:**
```markdown
0. Read Documentation (mandatory)
1. Read spec/plan
2. Design implementation
3. Write code
4. Write tests
5. Assess quality
6. Update Documentation (mandatory)
```

**Orchestrator skill phases:**
```markdown
1. Validate prerequisites (all skills installed)
2. Execute skill 1
3. (Optional) User review gate
4. Execute skill 2
5. (Optional) User review gate
6. Execute skill 3
7. Report completion
```

#### Step 3: Identify Quality Gates

**For each phase, determine:**
- What must be checked before proceeding?
- Where should user review happen?
- What can be automated vs manual?
- What are the pass/fail criteria?

**Examples:**

**After implementation phase:**
- Tests must pass (automated)
- Coverage ≥85% for production (automated)
- Code must pass modularity assessment (automated)
- (Optional) User code review (manual)

**After planning phase:**
- All requirements documented (checklist)
- Success criteria measurable (checklist)
- User approval obtained (manual)

#### Step 4: Customize for Repo Type

[See [references/checklist-management.md](../references/checklist-management.md) for conditional patterns]

**Add conditions for different repo types:**

**Prototype:**
- Lower thresholds (50% coverage)
- Skip security, modularity, performance
- Basic functionality only

**Production:**
- High thresholds (85% coverage)
- All quality gates
- Comprehensive checks

**Library:**
- Very high thresholds (90% coverage)
- API documentation required
- Backward compatibility checks
- Semver compliance

**Example checklist with repo-type customization:**
```markdown
- [ ] 1. Always: Run tests
- [ ] 2. Always: Check coverage ≥[50% prototype / 85% production / 90% library]
- [ ] 3. (If coverage below threshold) Add tests and return to step 1
- [ ] 4. (If production repo) Run security scans
- [ ] 5. (If production repo) Run modularity assessment
- [ ] 6. (If library) Verify API documentation complete
- [ ] 7. Always: Update ${SPEC_DIR}/[doc].md with progress
```

#### Step 5: Complete Design Template

**Fill out for each skill:**

```markdown
## Skill: [skill-name]

**Type:** [Planning / Implementation / Review / Orchestrator]

**Purpose:** [One sentence - what this skill does]

**Input:** [What this skill takes as input]

**Output:** [What this skill produces]

**Phases:**
0. (If implementation) Read Documentation
1. [Phase 1 name] - [Purpose] - Gate: [What must pass]
2. [Phase 2 name] - [Purpose] - Gate: [What must pass]
...
N. (If implementation) Update Documentation - Gate: [Docs updated]

**Quality gates:**
- After Phase X: [What to check]
- After Phase Y: [What to check]

**Repo type customization:**
- Prototype: [What to simplify - lower thresholds, skip gates]
- Production: [What to add - full quality checks]
- Library: [Special considerations - API, compatibility]

**References to framework docs:**
- Phase 0: References CLAUDE.md (if implementation)
- Checklists: Reference [checklist-management.md](../references/checklist-management.md)
- OKRs: Reference [goals-and-objectives.md](../references/goals-and-objectives.md)
- Assessments: Reference [assessment-approaches.md](../references/assessment-approaches.md)
- Progress updates: Reference [spec-maintenance.md](../references/spec-maintenance.md)
```

**Example completed design:**

```markdown
## Skill: create-api-endpoint

**Type:** Implementation

**Purpose:** Create a new API endpoint with tests and documentation

**Input:** Endpoint description (e.g., "GET /users/:id endpoint")

**Output:**
- API endpoint code
- Unit tests
- Integration tests
- API documentation

**Phases:**
0. Read Documentation - Gate: Conventions understood
1. Gather endpoint requirements - Gate: Requirements clear
2. Design endpoint structure - Gate: Design approved
3. Implement endpoint code - Gate: Code written
4. Write tests - Gate: Tests pass, coverage met
5. Assess quality - Gate: All quality checks pass
6. Update Documentation - Gate: API docs updated

**Quality gates:**
- After Phase 4: Tests pass, coverage ≥[threshold for repo type]
- After Phase 5: Security, modularity, conventions all pass
- After Phase 6: CLAUDE.md updated with new patterns (if any)

**Repo type customization:**
- Prototype: 50% coverage, skip security/modularity, basic tests only
- Production: 85% coverage, full security scan, modularity assessment
- Library: 90% coverage, API docs required, backward compatibility check

**References to framework docs:**
- Phase 0: Read CLAUDE.md
- Checklists: Use numbered items with conditions per checklist-management.md
- Phase 5: Run assessments per assessment-approaches.md
- All phases: Update progress per spec-maintenance.md
```

**Gate:** All skills designed with complete phase structure.

---

### Phase 3: Generate SKILL.md Files

**Purpose:** Create complete, copy-ready SKILL.md files following framework conventions.

[See [SETUP.md](../SETUP.md) for skill file structure]

**For each skill, generate complete SKILL.md using this template:**

```markdown
---
name: [skill-name]
description: [One-line description of what this skill does]
argument-hint: "[expected arguments or input]"
---

# [Skill Display Name]

[2-3 sentence description of what this skill does and when to use it]

---

## When to Use

**Use this skill when:**
- [Use case 1]
- [Use case 2]
- [Use case 3]

**When NOT to use:**
- [Anti-use case 1]
- [Anti-use case 2]

---

## Goal and OKRs

[See [references/goals-and-objectives.md](../references/goals-and-objectives.md) for OKR guidance]

**Goal:** [Single sentence - what to accomplish]

**OKRs:**
- Objective: [Aspirational outcome statement]
- Key Results:
  1. [Measurable outcome 1] [with threshold: ≥X%]
  2. [Measurable outcome 2] [with threshold: ≥X]
  3. [Measurable outcome 3] [binary: yes/no]

[See [references/assessment-approaches.md](../references/assessment-approaches.md) for assessment criteria]

---

## Prerequisites

**Required:**
- [Prerequisite 1]
- [Prerequisite 2]

**Optional:**
- [Optional prerequisite 1]

---

## Input

[What this skill takes as input - arguments, files, etc.]

**Format:**
- [Input format 1]
- [Input format 2]

---

## Output

[What this skill produces]

**Output files:**
- [File 1] at [location]
- [File 2] at [location]

---

## Workflow

### Phase 0: Read Documentation

[For implementation skills only - planning skills skip this]

**Purpose:** Understand project conventions before implementing.

**Read:**
- CLAUDE.md (project conventions, coding style, quality standards)
- [Other relevant project docs]

**Checklist:**

[See [references/checklist-management.md](../references/checklist-management.md) for checklist patterns]

- [ ] 1. Read CLAUDE.md and note repo type
- [ ] 2. Note coding conventions and style
- [ ] 3. Note quality thresholds for this repo type
- [ ] 4. (If planning skill calls another skill) Verify skill dependencies installed

**Gate:** Project conventions understood.

---

### Phase 1: [Phase Name]

**Purpose:** [What this phase accomplishes]

**Checklist:**

[See [references/checklist-management.md](../references/checklist-management.md) for conditional patterns]

- [ ] 1. Always: [Unconditional action]
- [ ] 2. (If production repo) [Production-specific action]
- [ ] 3. (If library) [Library-specific action]
- [ ] 4. (If [condition]) [Conditional action] and return to step 2
- [ ] 5. Always: Update ${SPEC_DIR}/[document].md with progress

[See [references/spec-maintenance.md](../references/spec-maintenance.md) for what to document]

**What to document in progress update:**
- Progress made in this phase
- Design changes or pivots
- Challenges encountered
- Solutions implemented
- Next steps

**Gate:** [What must pass to proceed to next phase]

---

### Phase 2: [Phase Name]

**Purpose:** [What this phase accomplishes]

**Checklist:**
- [ ] 1. [Action 1]
- [ ] 2. [Action 2]
- [ ] 3. (If [condition]) [Action with condition]
- [ ] 4. Always: Update ${SPEC_DIR}/[document].md with progress

**Gate:** [What must pass]

---

[Continue for all phases...]

---

### Phase N: [Implementation or Core Work Phase]

**Purpose:** [Main work of the skill]

**Checklist:**
- [ ] 1. [Core implementation steps]
- [ ] 2. (If production repo) Run quality checks
- [ ] 3. Always: Update ${SPEC_DIR}/[document].md with progress

**Quality assessment:**

[See [references/assessment-approaches.md](../references/assessment-approaches.md) for assessment criteria]

**(If implementation phase) Run assessments:**

**Always run:**
- Modularity/Maintainability (8 criteria + check for existing implementations)
- Convention/Best Practices
- Accuracy/Correctness

**(If production repo) Also run:**
- Security (vulnerability scans, input validation, secret management)
- Performance (response times, resource usage)
- Completeness (all requirements implemented)

**(If library) Also check:**
- API documentation complete
- Backward compatibility maintained
- Semantic versioning compliance

**Assessment checklist:**
- [ ] 1. Run modularity assessment
- [ ] 2. (If issues found) Refactor and return to step 1
- [ ] 3. Run convention check
- [ ] 4. (If violations) Fix and return to step 3
- [ ] 5. (If production repo) Run security scan
- [ ] 6. (If production repo AND vulnerabilities) Fix and return to step 5
- [ ] 7. (If library) Verify API docs complete
- [ ] 8. Always: Document assessment results in ${SPEC_DIR}/[document].md

**Gate:** All applicable assessments pass.

---

### Final Phase: Update Documentation

[For implementation skills only - planning skills skip this]

**Purpose:** Maintain project documentation with any new patterns or conventions.

**Update if needed:**
- CLAUDE.md (if new patterns introduced)
- API documentation (if public interfaces changed)
- CHANGELOG (if library with versioning)

**Checklist:**
- [ ] 1. (If new patterns) Update CLAUDE.md with new conventions
- [ ] 2. (If library AND API changes) Update API documentation
- [ ] 3. (If library AND user-facing changes) Update CHANGELOG
- [ ] 4. Always: Update ${SPEC_DIR}/[document].md with final progress entry

[See [references/spec-maintenance.md](../references/spec-maintenance.md) for what to document]

**Final progress entry should include:**
- Summary of all work completed
- All design changes made
- All challenges encountered and solutions
- Final quality assessment results
- Next steps or follow-up items

**Gate:** All documentation updated.

---

## Progress Tracking

After each phase, report progress using this format:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 [SKILL-NAME] PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PHASES:
✅ Phase 0: Read documentation
✅ Phase 1: [Phase name]
🔄 Phase 2: [Phase name] [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 3: [Phase name]
⏸️ Phase 4: [Phase name]

CURRENT TASK:
Phase 2: [What's currently happening]
Status: [Current status details]

CHECKLIST:
✅ [Completed item 1]
✅ [Completed item 2]
🔲 [Current/pending item]
🔲 [Future item]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Visual indicators:**
- ✅ = Completed
- 🔄 = In progress
- ⏸️ = Not started
- ◀── = Current position marker

---

## Customization by Repo Type

### Prototype Repos

**Simplifications:**
- Lower coverage thresholds (50% vs 85%)
- Skip security scans
- Skip modularity assessments
- Skip performance optimization
- Basic readability checks only

**Example checklist for prototype:**
```markdown
### Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests (coverage >50%)
- [ ] 2. Verify code works as expected
- [ ] 3. Check code is readable
- [ ] 4. (Skip for prototype) Security scans
- [ ] 5. (Skip for prototype) Modularity assessment
- [ ] 6. (Skip for prototype) Performance checks
- [ ] 7. Always: Update ${SPEC_DIR}/plan.md with progress
```

---

### Production Repos

**Full quality gates:**
- High coverage (≥85%)
- Security scans (bandit, safety, npm audit, etc.)
- Modularity assessments (all 8 criteria)
- Performance checks (<100ms response, <2s load)
- Comprehensive tests (unit + integration + e2e)

**Example checklist for production:**
```markdown
### Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests (all pass)
- [ ] 2. Check coverage ≥85%
- [ ] 3. (If coverage <85%) Add tests and return to step 1
- [ ] 4. Run security scans (bandit, safety, npm audit)
- [ ] 5. (If vulnerabilities found) Fix and return to step 4
- [ ] 6. Run modularity assessment (8 criteria)
- [ ] 7. (If modularity issues) Refactor and return to step 6
- [ ] 8. Check performance (<100ms response time)
- [ ] 9. (If performance issues) Optimize and return to step 8
- [ ] 10. Always: Update ${SPEC_DIR}/plan.md with progress, challenges, solutions
```

---

### Library Repos

**Extra requirements:**
- Very high coverage (≥90%, especially public APIs)
- All public APIs documented
- Backward compatibility maintained
- Semantic versioning compliance
- Migration guides for breaking changes

**Example checklist for library:**
```markdown
### Phase 5: Verify Implementation

**Checklist:**
- [ ] 1. Run tests (all pass)
- [ ] 2. Check coverage ≥90% (especially all public APIs)
- [ ] 3. (If coverage <90%) Add tests and return to step 1
- [ ] 4. Verify all public APIs have docstrings/documentation
- [ ] 5. (If documentation missing) Add docs and return to step 4
- [ ] 6. Check semantic versioning compliance
- [ ] 7. (If breaking changes) Write migration guide in CHANGELOG
- [ ] 8. Run backward compatibility tests
- [ ] 9. (If compatibility broken unintentionally) Fix and return to step 8
- [ ] 10. Verify examples in documentation still work
- [ ] 11. Always: Update ${SPEC_DIR}/plan.md with progress
```

---

## Best Practices

[Skill-specific best practices based on the skill's domain]

**General practices:**
- Follow project conventions in CLAUDE.md
- Document as you go (don't wait until end)
- Run quality checks incrementally (not all at end)
- Ask user for clarification when requirements unclear
- Update progress after each phase

---

## Anti-Patterns

**Avoid these common mistakes:**

❌ **Skipping Phase 0** (implementation skills)
- Always read CLAUDE.md first to understand conventions

❌ **Waiting to document progress**
- Update ${SPEC_DIR} docs after each phase, not at end

❌ **Implementing before understanding requirements**
- Complete planning phases thoroughly before implementation

❌ **Skipping quality gates for "speed"**
- Quality gates prevent rework and bugs

❌ **Not customizing for repo type**
- Prototype doesn't need 90% coverage, library does

❌ **Vague progress updates**
- Specific: "Added Redis cache (10x faster)", not "Made improvements"

---

## Related Skills

[If part of a workflow, list related skills]

**This skill is part of the [workflow-name] workflow:**
- [related-skill-1] - [What it does]
- [related-skill-2] - [What it does]
- [workflow-orchestrator] - Orchestrates all skills

**Alternative skills:**
- [alternative-skill] - Use when [different scenario]
```

**Save each skill as:** `${TOOL_CONFIG}/skills/[skill-name]/SKILL.md`

**Example:**
```bash
mkdir -p .claude/skills/create-api-endpoint
# Copy template above, customize for create-api-endpoint
# Save to .claude/skills/create-api-endpoint/SKILL.md
```

**Gate:** All SKILL.md files generated with complete framework conventions.

---

### Phase 4: Create Workflow Orchestrator (If Multi-Skill)

**Purpose:** If you designed a multi-skill workflow in Phase 2, create the orchestrator skill.

**Skip this phase if:** You created a single atomic skill.

**Orchestrator skill structure:**

```markdown
---
name: [workflow-name]-workflow
description: Orchestrates the complete [task] workflow
argument-hint: "[task-description or input-file]"
---

# [Workflow Name] Workflow

Orchestrates the complete [task] workflow by calling atomic skills in sequence with optional user review gates.

---

## When to Use

**Use this workflow when:**
- You want to complete the entire [task] process from start to finish
- You want automated handoff between planning and implementation phases

**When NOT to use:**
- You only need one phase (use atomic skills directly)
- You want manual control between phases

---

## Goal and OKRs

[See [references/goals-and-objectives.md](../references/goals-and-objectives.md) for OKR guidance]

**Goal:** Complete [task] workflow from [start] to [end] with quality gates

**OKRs:**
- Objective: Deliver complete, high-quality [output]
- Key Results:
  1. All workflow phases completed successfully (100%)
  2. All quality gates passed (100%)
  3. Output meets requirements (verified by user approval)

---

## Skills Called

This workflow orchestrates these atomic skills in sequence:

1. **[skill-1-name]** - [What it does] - Output: [what it produces]
2. **[skill-2-name]** - [What it does] - Output: [what it produces]
3. **[skill-3-name]** - [What it does] - Output: [what it produces]

---

## Prerequisites

**Required:**
- All atomic skills must be installed in ${TOOL_CONFIG}/skills/
- [Other prerequisites]

---

## Input

[What the workflow takes as input]

---

## Output

[What the complete workflow produces]

**Output files:**
- [File 1] from skill 1 at [location]
- [File 2] from skill 2 at [location]
- [File 3] from skill 3 at [location]

---

## Workflow

### Phase 0: Validate Prerequisites

**Purpose:** Ensure all atomic skills are installed before starting.

**Check for required skills:**
- ${TOOL_CONFIG}/skills/[skill-1-name]/SKILL.md
- ${TOOL_CONFIG}/skills/[skill-2-name]/SKILL.md
- ${TOOL_CONFIG}/skills/[skill-3-name]/SKILL.md

**Checklist:**
- [ ] 1. Verify all atomic skills exist
- [ ] 2. (If any missing) Error with installation instructions
- [ ] 3. Verify ${SPEC_DIR} directory exists and is writable

**Error message if skills missing:**
```markdown
❌ Required skills not found!

Missing skills:
- [skill-1-name]
- [skill-2-name]

To install, copy from skill-setup-guidelines/[category].md:
1. mkdir -p ${TOOL_CONFIG}/skills/[skill-name]
2. Copy SKILL.md template
3. Customize for your repo type

See setup-repo.md for complete installation guide.
```

**Gate:** All prerequisites validated.

---

### Phase 1: Execute [Skill 1 Name]

**Purpose:** [What skill 1 accomplishes in the workflow]

**Call skill:** `/[skill-1-name] [arguments]`

**Wait for completion.**

**Verify output:**
- [ ] 1. Skill completed successfully (check status)
- [ ] 2. Output file exists at [expected location]
- [ ] 3. Output is valid (not empty, correct format)

**If skill failed:**
- Review error message
- Fix issue
- Return to start of Phase 1

**Gate:** Skill 1 complete and output valid.

---

### Phase 2: (Optional) User Review - [Output from Skill 1]

**Purpose:** Let user review [output] before proceeding to implementation.

**Ask user:**
```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 REVIEW CHECKPOINT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Skill 1] has completed and produced:
[Output file] at [location]

Would you like to review this [output] before proceeding to [next phase]?

Options:
1. Yes - Pause for review
2. No - Continue automatically
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**If user chooses "Yes":**
- [ ] 1. Pause workflow
- [ ] 2. User reviews [output file]
- [ ] 3. Ask: "Is [output] approved to proceed?"
- [ ] 4. (If approved) Continue to Phase 3
- [ ] 5. (If not approved) Ask what needs to change, return to Phase 1 with changes

**If user chooses "No":**
- [ ] 1. Continue directly to Phase 3

**Gate:** User approved (or skipped review) and ready to proceed.

---

### Phase 3: Execute [Skill 2 Name]

**Purpose:** [What skill 2 accomplishes]

**Prepare arguments from Phase 1 output:**
- Input: [What from Phase 1 output to pass to Skill 2]

**Call skill:** `/[skill-2-name] [arguments from Phase 1]`

**Wait for completion.**

**Verify output:**
- [ ] 1. Skill completed successfully
- [ ] 2. Output file exists at [expected location]
- [ ] 3. Output is valid

**If skill failed:**
- Review error message
- Determine if issue is in Skill 2 or input from Skill 1
- Fix and retry

**Gate:** Skill 2 complete and output valid.

---

[Continue pattern for all skills in workflow...]

---

### Phase N: Execute [Final Skill]

**Purpose:** [What final skill accomplishes]

**Call skill:** `/[skill-N-name] [arguments]`

**Wait for completion.**

**Verify output:**
- [ ] 1. Skill completed successfully
- [ ] 2. All outputs produced
- [ ] 3. All quality gates passed

**Gate:** Final skill complete.

---

### Final Phase: Report Completion

**Purpose:** Summarize workflow results and next steps.

**Report:**

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ [WORKFLOW NAME] COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WORKFLOW SUMMARY:
✅ Phase 1: [Skill 1] - [Brief result]
✅ Phase 2: [Skill 2] - [Brief result]
✅ Phase 3: [Skill 3] - [Brief result]

OUTPUTS PRODUCED:
- [Output 1] at [location]
- [Output 2] at [location]
- [Output 3] at [location]

QUALITY CHECKS PASSED:
✅ [Quality check 1]
✅ [Quality check 2]
✅ [Quality check 3]

NEXT STEPS:
- [What user should do next]
- [Any follow-up actions]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Gate:** Workflow complete, user informed of results.

---

## Progress Tracking

Show workflow progress:

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 [WORKFLOW-NAME] PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WORKFLOW PHASES:
✅ Phase 0: Validate prerequisites
✅ Phase 1: Execute [skill-1] → [output produced]
✅ Phase 2: User review → [approved/skipped]
🔄 Phase 3: Execute [skill-2] [IN PROGRESS] ◀── YOU ARE HERE
⏸️ Phase 4: Execute [skill-3]
⏸️ Phase 5: Report completion

CURRENT SKILL: [skill-2-name]
Status: [What skill 2 is currently doing]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Error Handling

**If atomic skill fails:**

1. Capture error from skill
2. Display error to user
3. Suggest fixes based on error type
4. Ask if user wants to retry or abort workflow

**Common errors:**

**Skill not found:**
```markdown
❌ Error: Skill '[skill-name]' not found

Install with:
mkdir -p ${TOOL_CONFIG}/skills/[skill-name]
# Copy SKILL.md from skill-setup-guidelines/
```

**Skill prerequisites not met:**
```markdown
❌ Error: [Skill-name] requires [prerequisite]

Fix: [How to resolve]
Then retry workflow.
```

**Quality gate failed:**
```markdown
❌ Error: [Skill-name] quality gate failed

Issue: [What failed]
Threshold: [Expected value]
Actual: [Actual value]

Options:
1. Fix issue and retry
2. (If prototype) Lower threshold
3. Abort workflow
```
```

**Save orchestrator as:** `${TOOL_CONFIG}/skills/[workflow-name]-workflow/SKILL.md`

**Gate:** Orchestrator created (if multi-skill workflow).

---

### Phase 5: Test Skills

**Purpose:** Verify all skills work correctly in your repository.

**For each atomic skill:**

**Test checklist:**
- [ ] 1. Test invocation (`/[skill-name]` or direct execution)
- [ ] 2. Verify skill can read CLAUDE.md
- [ ] 3. Verify skill can write to ${SPEC_DIR}
- [ ] 4. Check progress tracking displays correctly
- [ ] 5. Verify framework reference links work (references/ docs)
- [ ] 6. Test repo-type customization (correct thresholds/gates)
- [ ] 7. Run through complete skill workflow with test data
- [ ] 8. Verify output matches expected format

**Example test for create-api-endpoint:**

```bash
# Test: Create simple API endpoint
/create-api-endpoint "GET /health endpoint that returns 200 OK"

# Expected behavior:
# 1. Reads CLAUDE.md (shows repo type, conventions)
# 2. Interviews for details (path, method, response)
# 3. Creates endpoint code in correct location
# 4. Creates tests with appropriate coverage for repo type
# 5. Runs quality checks (appropriate for repo type)
# 6. Updates progress in specs/api-endpoint-plan.md
# 7. Shows completion summary

# Verify outputs:
ls -la src/api/health.py         # Endpoint code
ls -la tests/api/test_health.py  # Tests
cat specs/api-endpoint-plan.md   # Progress log

# Check quality:
# - Code follows CLAUDE.md conventions?
# - Tests exist and pass?
# - Coverage meets threshold for repo type?
# - Progress documented?
```

**For workflow orchestrator (if applicable):**

**Test end-to-end workflow:**
- [ ] 1. Test invoking workflow with test input
- [ ] 2. Verify prerequisite validation works (try with missing skill)
- [ ] 3. Verify skills called in correct order
- [ ] 4. Verify outputs passed between skills correctly
- [ ] 5. Test user review gates (try both approve and reject)
- [ ] 6. Verify error handling (force a skill failure)
- [ ] 7. Check final summary report
- [ ] 8. Verify all expected outputs produced

**Example test for migration-workflow:**

```bash
# Test: Database migration workflow
/migration-workflow "Add user_preferences table"

# Expected workflow:
# 1. Validates migration-plan and migration-execute skills exist
# 2. Calls migration-plan skill
#    - Creates migration design in specs/migration-plan.md
#    - Shows plan to user
# 3. User review gate: "Approve plan?"
#    - Test: Approve
# 4. Calls migration-execute skill
#    - Backs up database
#    - Tests migration on copy
#    - Applies to production
#    - Verifies success
# 5. Shows completion summary

# Verify:
cat specs/migration-plan.md      # Has plan
ls -la migrations/001_user_preferences.sql  # Migration file
cat specs/migration-log.md       # Execution log
# Database has new table? (manual check)
```

**Common issues and fixes:**

| Issue | Cause | Fix |
|-------|-------|-----|
| Skill not found | Not installed in ${TOOL_CONFIG}/skills/ | Create directory and add SKILL.md |
| Can't read CLAUDE.md | Wrong path or file doesn't exist | Create CLAUDE.md in repo root |
| Can't write to ${SPEC_DIR} | Directory doesn't exist or no permissions | Create directory, check permissions |
| Wrong thresholds | Not customized for repo type | Review Phase 6 of setup-repo.md |
| References broken | Wrong paths to references/ docs | Check relative paths in SKILL.md |
| Workflow stops | Missing prerequisite | Install missing skill |

**Gate:** All skills tested and working correctly.

---

### Phase 6: Document Created Skills

**Purpose:** Record what was created for future reference and maintenance.

**Update CLAUDE.md:**

```markdown
## Custom Skills

### [Skill/Workflow Name]

**Purpose:** [One-line description]

**Type:** [Single skill / Multi-skill workflow]

**Location:** ${TOOL_CONFIG}/skills/[skill-name]/

**Usage:** `/[skill-name] [arguments]`

**Created:** 2026-02-15

**Customizations:**
- Repo type: [Prototype/Production/Library]
- Coverage threshold: [50%/85%/90%]
- Quality gates: [Which gates enabled]

**(If workflow) Skills included:**
- [skill-1] - [Purpose]
- [skill-2] - [Purpose]
- [orchestrator] - Coordinates workflow

**Testing:**
- Tested: 2026-02-15
- Test case: [Brief description of test]
- Result: ✅ Working as expected

**Maintenance notes:**
- [Any special considerations]
- [Known limitations]
- [Future improvements planned]
```

**Create skill documentation file (optional but recommended):**

```bash
# Create detailed skill docs
cat > specs/SKILLS-DOCUMENTATION.md << 'EOF'
# Custom Skills Documentation

## [Skill Name]

### Overview
[Detailed description]

### When to Use
[Scenarios where this skill is appropriate]

### Input Format
[Detailed input specification]

### Output Format
[Detailed output specification]

### Examples

#### Example 1: [Scenario]
```bash
/[skill-name] [example-input]
```

**Expected output:**
- [File 1] with [contents]
- [File 2] with [contents]

#### Example 2: [Scenario]
```bash
/[skill-name] [example-input-2]
```

**Expected output:**
- [Different output]

### Troubleshooting

**Issue: [Common problem]**
- Cause: [Why it happens]
- Fix: [How to resolve]

### Maintenance

**Last updated:** 2026-02-15
**Maintained by:** [Team/person]
**Review frequency:** [Monthly/quarterly]

EOF
```

**Gate:** Skills documented in CLAUDE.md and/or dedicated docs.

---

## Examples

### Example 1: Single Skill (API Endpoint Creation)

**Scenario:** "I want a skill for creating API endpoints following our conventions"

#### Phase 1: Define Purpose

**Problem:** API endpoint creation is inconsistent
**Solution:** Single atomic skill
**Decision:** create-api-endpoint (Implementation skill)

#### Phase 2: Design Structure

**Type:** Implementation
**Phases:**
- 0: Read CLAUDE.md
- 1: Gather endpoint requirements (interview)
- 2: Design endpoint structure
- 3: Implement endpoint code
- 4: Write tests
- 5: Assess quality
- 6: Update documentation

**Quality gates:**
- Tests pass with coverage ≥[threshold]
- Security scan passes (production only)
- Modularity assessment passes (production only)

#### Phase 3: Generate SKILL.md

Created `.claude/skills/create-api-endpoint/SKILL.md` with:
- Goal: Create production-ready API endpoint
- OKRs: Endpoint works, tests ≥85%, security/modularity pass
- 7 phases (0-6)
- Checklists with repo-type conditions
- References to all framework docs

#### Phase 4: Orchestrator

N/A - Single skill, no orchestrator needed

#### Phase 5: Test

```bash
/create-api-endpoint "GET /users/:id endpoint"
# ✅ Created endpoint code
# ✅ Created tests (87% coverage)
# ✅ Passed security scan
# ✅ Passed modularity assessment
```

#### Phase 6: Document

Updated CLAUDE.md:
```markdown
## Custom Skills

### create-api-endpoint
**Purpose:** Create RESTful API endpoints with tests
**Usage:** `/create-api-endpoint "description"`
**Created:** 2026-02-15
```

---

### Example 2: Multi-Skill Workflow (Database Migration)

**Scenario:** "I want a workflow for database migrations with safety checks"

#### Phase 1: Define Purpose

**Problem:** Migrations are ad-hoc and risky
**Solution:** Multi-skill workflow
**Decision:** 3 skills needed:
1. migration-plan (Planning)
2. migration-execute (Implementation)
3. migration-workflow (Orchestrator)

#### Phase 2: Design Structure

**Skill 1: migration-plan**
- Type: Planning
- Phases: Analyze schema → Design migration → Test plan → Output plan
- Output: specs/migration-plan.md

**Skill 2: migration-execute**
- Type: Implementation
- Phases: 0=Read docs → Read plan → Backup → Test on copy → Apply → Verify → 6=Update docs
- Output: Migration applied + log

**Skill 3: migration-workflow**
- Type: Orchestrator
- Phases: Validate → Call plan → User review → Call execute → Report

#### Phase 3: Generate SKILL.md Files

Created three SKILL.md files:

1. `.claude/skills/migration-plan/SKILL.md`
   - Goal: Design safe migration
   - OKRs: Plan complete, rollback plan exists, tested on copy
   - 4 phases
   - Output: migration-plan.md

2. `.claude/skills/migration-execute/SKILL.md`
   - Goal: Execute migration safely
   - OKRs: Backup created, migration succeeds, rollback tested
   - 7 phases (includes Phase 0 and Final Phase)
   - Quality gates: Backup verification, test on copy, production verification

3. `.claude/skills/migration-workflow/SKILL.md`
   - Goal: Complete migration from planning to execution
   - OKRs: All phases complete, zero data loss, rollback available
   - 5 phases: Validate → Plan → Review → Execute → Report
   - User review gate between plan and execute

#### Phase 4: Create Orchestrator

migration-workflow orchestrator:
- Validates both atomic skills exist
- Calls migration-plan first
- Pauses for user review of plan
- Calls migration-execute only if approved
- Shows comprehensive completion summary

#### Phase 5: Test

```bash
# Test end-to-end
/migration-workflow "Add user_preferences table"

# Phase 1: migration-plan executes
# → Creates specs/migration-plan.md
# → Shows plan: Schema changes, indexes, constraints

# Phase 2: User review
# → "Approve plan?" → Yes

# Phase 3: migration-execute runs
# → Backs up database (verified)
# → Tests on copy (success)
# → Applies to production (success)
# → Verifies data (success)

# Result:
# ✅ Migration complete
# ✅ Backup at backups/db-2026-02-15.sql
# ✅ Log at specs/migration-log.md
```

#### Phase 6: Document

Updated CLAUDE.md:
```markdown
## Custom Skills

### Database Migration Workflow

**Purpose:** Safely plan and execute database migrations
**Type:** Multi-skill workflow
**Location:** ${TOOL_CONFIG}/skills/migration-*
**Usage:** `/migration-workflow "description"`
**Created:** 2026-02-15

**Skills included:**
- migration-plan - Design migration and safety plan
- migration-execute - Execute with backup and verification
- migration-workflow - Orchestrates both with review gate

**Customizations:**
- Always backs up before migration
- Always tests on copy first
- User review required before production apply

**Testing:**
- Tested: 2026-02-15
- Test: Added test table to development database
- Result: ✅ Success (with rollback test)
```

---

## Summary

This guide helps you create:

✅ **Single atomic skills** - For specific tasks (create endpoint, run migration, etc.)

✅ **Multi-skill workflows** - Planning + Implementation + Orchestrator patterns

✅ **Framework-compliant skills** - Following all conventions:
- Goal and OKRs (outcome-oriented, measurable)
- Checklists with conditions (repo-type aware)
- Progress tracking (after each phase)
- Quality assessments (appropriate for repo type)
- References to framework docs (no duplication)

✅ **Customized for repo type** - Prototype/Production/Library thresholds

**All skills include:**
- [references/goals-and-objectives.md](../references/goals-and-objectives.md) - OKR patterns
- [references/checklist-management.md](../references/checklist-management.md) - Checklist format
- [references/assessment-approaches.md](../references/assessment-approaches.md) - Quality criteria
- [references/spec-maintenance.md](../references/spec-maintenance.md) - Progress documentation
- Phase 0: Read Documentation (implementation skills)
- Final Phase: Update Documentation (implementation skills)

**Next steps after creating skills:**
1. Test thoroughly with real use cases
2. Iterate based on experience
3. Update CLAUDE.md as conventions evolve
4. Share skills with team if applicable

**Getting help:**
- Skill not working? Review Phase 5 testing checklist
- Need to understand framework patterns? Read [references/](../references/)
- Need to set up repository first? See [setup-repo.md](setup-repo.md)
