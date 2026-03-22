---
name: setup-ai-env
description: >
  Configure development environment for AI-readiness.
  Creates CLAUDE.md files, .claude/ infrastructure, task location.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup ai environment", "setup ai env", "create CLAUDE.md", "set up ai project".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion
---

# Setup AI Env

**Goal:** Configure an AI-ready development environment.

**Intent:** AI assistants work effectively when they have context. This workflow creates the infrastructure that provides that context.

**Role:** AI Environment Consultant

A specialist in configuring projects for effective AI-assisted development. Understands how AI assistants consume context, what information they need to work effectively, and how to structure that information across CLAUDE.md files and supporting infrastructure.

**Scope:** Adapt the user's environment to be AI-ready. This includes anything that will improve AI-readiness, but not more.

**AI-Ready Definition:** A project configuration where AI assistants have the context they need to work effectively. This includes:
- **Project context** — type, phase, conventions documented in CLAUDE.md
- **Interaction preferences** — autonomy level, communication style
- **Infrastructure for improvement** — feedback tracking, maintenance triggers
- **Tool configuration** — permissions, MCP servers

**Out of scope:** Source code structure, test configs, build files, dependencies, skill creation (handled by setup-skill-builder).

**Workflow Style:** Goal-Oriented (Declarative).

---

## Key Results - KR

1. CLAUDE.md files created with project context and AI profile
2. .claude/ infrastructure established
3. Task infrastructure configured
4. Environment validated against AI Environment Components

## Requirements and Constraints - REQ

1. Read task.md if available — use as starting context, but validate independently
2. Read `interaction-modes.md` and apply Interaction Mode throughout
3. Track progress per `tracking_and_recovery.md`
4. Reference AI Environment Components table when designing
5. STRICTLY in scope: Only execute actions that are in line with creating an AI-read environment (e.g. CLAUDE.md files, .claude/ directory, task location)

## Preconditions

**Required:** None — skill discovers what's needed

**Optional:**
- task.md from setup-init (provides starting context)
- Existing project structure

## Postconditions

**Success:** Environment is set up to be AI-Ready

**Failure:** Trace documents what was attempted and blockers

---

## Steps

1. Create preliminary checklist → KR1-4. Use TodoWrite: `setup-ai-env - KR# - <action>`
2. Plan execution → Based on context, output proposed actions for approval
3. {Placholder: Execute actions, add multiple steps to complete all key results successfully after creating or revising the plan (step 2 or any other)} → KR?
4. Validate KRs → Output positive and negative evidence for each

---

## Actions

### A0: Set Plan (→ KR1, KR2, KR3, KR4)

Intent: Establish approach using expert reasoning before diving into work
KR: Plan with priorities and unknowns documented
Preconditions:
- Required: Access to project directory
Postconditions:
- Success: Output plan with priorities, unknowns identified
- Failure: Output what's blocking planning
Exit Conditions:
- Cannot access project → stop, request access

Objectives:
1. Output a plan that will lead to setting up the environment successfully
2. Keep the plan updated as new knowledge is acquired

Constraints:
1. You MUST identify uncertainties
2. You MUST output how an expert will tackle this problem.
3. You MUST output priorities at a business/user level (no design details)
4. You MUST NOT go beyond the scope
5. You MUST follow your Interaction Mode

---

### A1: Review Materials (→ KR1, KR2, KR3)

Intent: Understand current state before making recommendations
KR: Findings documented with implications
Preconditions:
- Required: Plan from A0
Postconditions:
- Success: Output findings and what they imply for environment design
- Failure: Output what could not be accessed
Exit Conditions:
- No project access → stop, request access

Objectives:
1. Output findings for each AI Environment Component
2. Output implications for environment design
3. Revise plan (A0) if findings surface new priorities or unknowns

Constraints:
1. You MUST read task.md if it exists
2. You MUST examine project structure (languages, frameworks, conventions)
3. You MUST check for existing .claude/, CLAUDE.md files
4. You MUST share findings before drawing conclusions
5. Read `elicitation.md` and apply Inference First

---

### A2: Request Materials (→ KR1, KR2, KR3)

Intent: Identify and request missing information needed for good recommendations
KR: All required context obtained or gaps documented
Preconditions:
- Required: Findings from A1
Postconditions:
- Success: Output additional materials received
- Failure: Output what remains missing, proceed with defaults
Exit Conditions:
- Developer declines to provide → document gaps, apply defaults

Objectives:
1. All required context obtained for each AI Environment Component
2. Gaps documented with rationale

Constraints:
1. You MUST reason about what's missing before requesting
2. You MUST use AskUserQuestion with rationale, not bare questions
3. You MUST adapt request style to Interaction Mode
4. Read `elicitation.md` and apply Targeted Inquiry

---

### A3: Research Best Practices (→ KR1, KR2)

Intent: Ensure recommendations reflect current best practices
KR: Best practices researched and documented
Preconditions:
- Required: Understanding of project type from A1/A2
Postconditions:
- Success: Output relevant best practices for this project type
- Failure: Output what could not be researched, proceed with known practices
Exit Conditions:
- WebSearch unavailable → proceed with known practices

Objectives:
1. Best practices documented for this project type
2. Relevant MCP servers identified
3. Findings output for AI Environment Components

Constraints:
1. You MUST use WebSearch to find current practices
2. You MUST focus on AI assistant effectiveness, not general dev practices

---

### A4: Interview About Intent (→ KR1, KR2)

Intent: Understand what developer wants from AI assistance
KR: Developer intent documented, Interaction Mode confirmed
Preconditions:
- Required: Materials from A1/A2, research from A3
Postconditions:
- Success: Output intent summary with Interaction Mode
- Failure: Output partial understanding, apply defaults
Exit Conditions:
- Developer provides explicit direction → follow it (especially in Junior mode)

Objectives:
1. Developer intent documented
2. Interaction Mode confirmed
3. Project type and phase clarified

Constraints:
1. You MUST offer recommendations, not just questions
2. You MUST adapt interview depth to Interaction Mode
3. Read `pragmatics.md` and apply Recommended Option
4. Read `interaction-modes.md`

---

### A5: Propose Design (→ KR1, KR2, KR3, KR4)

Intent: Propose complete AI environment design
KR: Design proposal covers all AI Environment Components
Preconditions:
- Required: Intent from A4
Postconditions:
- Success: Output proposal, obtain approval per Interaction Mode
- Failure: Output proposal, iterate based on feedback
Exit Conditions:
- Developer rejects 3 times → stop, escalate

Objectives:
1. Design proposal covering all AI Environment Components
2. Approval obtained per Interaction Mode

Constraints:
1. You MUST reference AI Environment Components table
2. You MUST determine CLAUDE.md placement (root + package directories)
3. You MUST propose with rationale
4. You MUST adapt proposal style to Interaction Mode
5. You MUST follow Proposal Template from `setup-env-templates.md`
6. Read `pragmatics.md` and apply Recommended Option
7. Read `interaction-modes.md`

---

### A6: Implement Environment (→ KR1, KR2, KR3, KR4)

Intent: Create all approved artifacts
KR: Implementation matches approved design, no gaps
Preconditions:
- Required: Approved design from A5
Postconditions:
- Success: Output all artifacts created with locations
- Failure: Output errors, partial implementation documented
Exit Conditions:
- Design not approved → stop, complete A5 first
- Critical error → stop, report and await guidance

Objectives:
1. All approved artifacts created
2. Implementation matches approved design
3. Summary output of what was created and what was intentionally not created

Constraints:
1. You MUST validate design completeness against AI Environment Components
2. You MUST identify gaps before implementing
3. You MUST NOT create: src/, tests/, config files, language-specific files
4. You MUST confirm before overwriting existing files
5. You MUST configure worktrees if requested (per `worktree-setup-guideline.md`)
6. Read `discipline.md` and apply Sequential Processing

---

### A7: Evaluate Setup (→ KR4)

Intent: Verify environment matches design and is complete
KR: All AI Environment Components verified
Preconditions:
- Required: Implementation from A6
Postconditions:
- Success: Output verification with evidence for each component
- Failure: Output failures with remediation steps
Exit Conditions:
- Critical failure → stop, await guidance

Objectives:
1. All AI Environment Components verified against design
2. Environment confirmed AI-Ready
3. Hand off: "Environment is AI-Ready. Run `/setup-skill-builder` to continue."

Constraints:
1. You MUST verify each component with evidence (file exists, contains X)
2. You MUST confirm CLAUDE.md files contain required sections
3. You MUST confirm .claude/ structure is complete
4. Read `software_quality.md` and apply Correctness

---

## Additional Notes and Terms

**AI Environment Components:**

| Component | Purpose | Typical Contents |
|-----------|---------|------------------|
| `/CLAUDE.md` | Root AI context | Project type, phase, Interaction Mode, AI personality, coding conventions |
| `/{dir}/CLAUDE.md` | Directory context | Package-specific conventions, module purpose, local patterns |
| `/CLAUDE.local.md` | Personal settings | Local paths, personal preferences (gitignored) |
| `/.claude/` | AI infrastructure | Skills, feedback, settings |
| `/.claude/skills/` | Skill definitions | SKILL.md files for each skill |
| `/.claude/feedback/` | Improvement tracking | README.md, template.md, collected feedback |
| `/.claude/settings.json` | Permissions & MCPs | Tool permissions, MCP configurations |
| `/{tasks}/` | Task infrastructure | Task folders with task.md, notes, artifacts |
| `/{docs}/` | Document infrastructure | System documentation. |

**CLAUDE.md Placement Logic:**
- Root CLAUDE.md: Always created
- Directory CLAUDE.md: Created in package directories (detected by package.json, pyproject.toml, Cargo.toml, go.mod, etc.)
- CLAUDE.local.md: Template created, gitignored

**Templates:** See `setup-env-templates.md` for:
- Proposal Template
- CLAUDE.md Example
- Personality Translation Reference
- Feedback Template

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Overwriting existing files | CLAUDE.md exists | Confirm before overwriting |
| 2 | Incomplete design | Components missed | Validate against AI Environment Components table |
| 3 | Interaction Mode mismatch | Wrong autonomy level | Confirm mode in A4, reference throughout |
| 4 | Scope creep | Creating non-AI files | Validate only .claude/* and CLAUDE.md files created |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [interaction-modes.md](../../protocols/interaction-modes.md)
- [setup-env-templates.md](setup-env-templates.md)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
