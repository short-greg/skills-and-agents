---
name: setup-init
description: >
  Consultative workflow to understand a project and plan AI-readiness setup.
  Interviews developer, establishes interaction mode, defines criteria, outputs implementation plan.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "set up AI environment", "make project AI-ready", "setup project", "setup init".
argument-hint: "[project path or context]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup Init

**Goal:** Understand project and team needs, establish interaction mode, define AI-readiness criteria, output approved implementation plan.

**Intent:** Projects differ in structure, conventions, and needs. Before implementing anything, understand what the developer wants and how they want to collaborate with AI.

**Scope:** Consultation from assessment through plan approval. No implementation — that's handled by setup-env and setup-skill-builder workflows.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach to project needs.


---

## Key Results - KR

1. Interaction Mode established (Lead/Senior/Peer/Junior) and documented
2. Project's current state and AI-readiness gaps documented
3. Project-specific definition of "AI-ready" agreed with developer
4. Implementation plan approved and task.md created with all decisions

## Requirements and Constraints - REQ

1. Track progress per `tracking_and_recovery.md` — use both TodoWrite and trace file with event records (list of events with summaries of what was requested)
2. Use AskUserQuestion for all interview questions [COMMENT: This only needs to be mentioned once here]

**Event Record Structure:**
- Event type: [step_start / step_complete / decision_made / user_response / user_question_asked]
- Timestamp: [when it occurred]
- Summary: [what was requested or accomplished]
- Artifacts: [files created/modified, decisions made, if applicable]

**Event Examples:**
- `step_start: Create Interview Plan - 10:23 AM - Starting interview plan creation based on project assessment`
- `user_response: Interview Plan Confirmation - 10:28 AM - Developer approved plan with all 8 topics, no modifications requested`
- `decision_made: Interaction Mode Selected - 10:35 AM - Developer chose Peer mode (Recommended). Reasoning: moderate autonomy for collaborative work`
- `step_complete: Assess and Gather Requirements - 10:45 AM - Completed. Artifacts: Interaction Mode (Peer), AI Personality profile (5 traits), Communication preferences documented`
2. Ensure recoverable from interruption — check for existing trace on startup
3. Consultative not prescriptive — recommend with rationale, developer decides
4. Establish Interaction Mode early — it determines how rest of consultation proceeds
5. Iterate up to 3 times per action if developer requests changes before escalating
6. Use AskUserQuestion tool for multi-choice questions (Interaction Mode, personality traits, priorities)

## Preconditions

**Required:** Access to project repository

**Optional:**
- Project context (what it does, tech stack)
- Team context (solo/team, AI tool experience)
- Current pain points with AI assistance
- Existing CLAUDE.md or conventions documentation

## Postconditions

**Success:**
- task.md created at `tasks/{TASK_ID}/task.md` documenting all decisions
- Interaction mode, assessment, definition, and plan all documented
- setup-env installed to `.claude/skills/`

**Failure:** Trace documents what was attempted and blockers; partial deliverables noted

---

## Steps

1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with formula: `interview - KR# - <action> - <details>`
2. Check for existing trace → Execute A1 if trace exists
3. Assess project → KR2. Execute A2. Output assessment summary
4. Plan interview → KR1-4. Execute A3. Output interview plan for approval
5. Conduct interview → KR1, KR2, KR3. Execute A4
6. Create task.md → KR3, KR4. Execute A5. Document all decisions
7. Install setup-ai-env → Copy `workflows/setup/setup-ai-env.md` to `.claude/skills/setup-ai-env/SKILL.md`
8. Output completion summary → Inform user: "Setup init complete. Run `/setup-ai-env tasks/{TASK_ID}/task.md` to continue."

---

## Actions

Actions are units of work that apply protocol techniques to achieve specific outcomes.

### A1: Recover from Interruption (→ KR1, KR2, KR3, KR4)

Intent: Resume consultation with full context after session interruption
KR: Recovery summary and confirmed resume point documented
Preconditions:
- Required: Trace file from previous session
Postconditions:
- Success: Output recovery summary, output confirmed resume point
- Failure: Output trace status, output request for user guidance
Exit Conditions:
- Trace shows workflow completed → stop, inform user
- Trace shows mid-step → stop, ask user how to proceed

Instructions:
1. Read `tracking_and_recovery.md` and apply Trace Detection — to locate previous session state
2. Read `pragmatics.md` and apply Context Reconstruction — to rebuild understanding
3. Apply Resume Decision — to determine where to continue
4. Output recovery summary for user confirmation

---

### A2: Assess Project (→ KR2)

Intent: Understand project state and identify AI-readiness considerations
KR: Assessment summary with findings documented
Preconditions:
- Required: Repository access
- Optional: Existing documentation
Postconditions:
- Success: Output assessment summary with tech stack, conventions, and gaps
- Failure: Output partial findings, output request for access to missing areas
Exit Conditions:
- Repository inaccessible → stop, request access
- No code or documentation found → stop, clarify project state with user

Objectives (OBJ):
1. Identify project tech stack, structure, and conventions
2. Surface AI-readiness gaps and considerations
3. Document findings in assessment summary

Constraints (CONST):
1. Read `pragmatics.md` and apply Context Assessment — to understand project state
2. Focus on AI-readiness considerations, not general code review
3. Infer from context before asking user questions

Validation:
1. Add OBJ1-3 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A3: Plan Interview (→ KR1, KR2, KR3, KR4)

Intent: Determine relevant interview topics and create approach
KR: Interview plan with topics and rationale approved by developer
Preconditions:
- Required: Assessment summary
Postconditions:
- Success: Output interview plan with topics and rationale
- Failure: Output partial plan, output request for missing assessment information
Exit Conditions:
- Assessment incomplete → stop, complete A2 first
- Developer rejects plan 3 times → stop, escalate

Instructions:
1. Read `thinking.md` and apply Strategic Thinking — to plan interview approach
2. Read `risk_management.md` and apply Fail-Fast Ordering — to prioritize critical topics first
3. Review topics in Additional Notes, select relevant ones based on assessment
4. For each included topic, explain why it matters for this project
5. Output interview plan for developer confirmation

---

### A4: Conduct Interview (→ KR1, KR2, KR3)

Intent: Gather developer preferences and requirements through effective questioning
KR: Interaction Mode established, AI Personality profiled, AI-readiness criteria documented
Preconditions:
- Required: Approved interview plan
Postconditions:
- Success: Output all interview responses with Interaction Mode, AI Personality profile, and criteria
- Failure: Output partial responses, output blockers identified
Exit Conditions:
- Developer requests to stop → stop, document what was gathered, note incomplete
- Critical information refused → stop, document assumption to use defaults

Objectives (OBJ):
1. Establish Interaction Mode (Lead/Senior/Peer/Junior)
2. Profile AI Personality using Big 5 framework
3. Discover AI-readiness criteria and priorities
4. Respect developer's time — infer before asking

Constraints (CONST):
1. Read `elicitation.md` and apply Inference Before Asking — to minimize questions
2. Use AskUserQuestion tool for multi-choice questions
3. Follow interview plan topics
4. Do not exceed 3 follow-up questions per topic

Validation:
1. Add OBJ1-4 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A5: Create Task Documentation (→ KR3, KR4)

Intent: Document all decisions and create implementation plan
KR: task.md created with AI-readiness definition and actionable implementation plan
Preconditions:
- Required: Interview responses with Interaction Mode, AI Personality, constraints
Postconditions:
- Success: Output task.md file at `tasks/{TASK_ID}/task.md` with all sections populated
- Failure: Output partial task.md, output list of missing sections
Exit Conditions:
- Interview responses incomplete → stop, complete A4 first
- Cannot create tasks directory → stop, report filesystem error

Instructions:
1. Read `criteria_setting.md` and apply Operational Definition — to define "AI-ready"
2. Read `goal_setting.md` and apply Goal Decomposition — to create implementation plan
3. Create `tasks/{TASK_ID}/` directory
4. Write task.md following the template below
5. Verify all sections populated (use "Not specified" if developer skipped)
6. Output confirmation with file path

**task.md Structure:**

```markdown
# Setup Interview Results - {TASK_ID}

Date: {date}
Interview completed by: Claude (setup-interview workflow)

## Project Assessment

**Tech Stack:**
[Detected tech stack]

**Project Type:**
[Type and characteristics]

**Current State:**
[What exists, what works, what doesn't]

**Conventions and Patterns:**
[Detected conventions, patterns, anti-patterns]

**AI-Readiness Gaps:**
[What's missing for effective AI assistance]

## AI-Readiness Criteria

**Definition of "AI-Ready" for this project:**
[Project-specific definition agreed with developer]

**Priority Dimensions:**
[Which dimensions matter most: understanding, navigation, behavior knowledge, etc.]

**Success Metrics:**
[How will we know AI-readiness is achieved]

## Implementation Plan

**Environment Setup (for setup-env workflow):**
- [ ] [Task 1]
- [ ] [Task 2]
...

**Skills Setup (for setup-skill-builder workflow):**
- [ ] [Task 1]
- [ ] [Task 2]
...

**Sequencing:**
[Which must happen first, any dependencies]

## Constraints and Preferences

**Timeline:** [If specified]
**Budget:** [If specified]
**Compatibility Requirements:** [If specified]
**Style Preferences:** [If specified]
**Other Constraints:** [Any other discovered constraints]

## Notes

[Any additional context or decisions made during interview]
```

**Validation:**
After creating, verify:
- [ ] File exists at tasks/{TASK_ID}/task.md
- [ ] All sections populated (use "Not specified" if developer skipped)
- [ ] Interaction Mode clearly documented
- [ ] AI Personality profile complete
- [ ] Implementation plan actionable

---

## Additional Notes and Terms

**Interview Topics:**

Determine relevance based on project assessment. Not all topics apply to every project.

**Core Topics** (typically include, but can skip if developer requests):
- **Interaction Mode:** How developer wants to collaborate with AI (Lead/Senior/Peer/Junior)
- **AI Personality:** Developer's preferred AI characteristics (see below)
- **Communication Style:** Formality, verbosity, technical level, tone
- **Success Criteria:** How to measure AI-readiness

**Interaction Modes:**
- **Lead:** AI almost entirely autonomous, developer expects good output without much review
- **Senior:** AI fairly autonomous, receives some feedback, less collaboration than Peer
- **Peer:** AI and developer collaborate heavily on designs and plans
- **Junior:** Developer heavily reviews AI output, confirms designs/plans, AI does not jump to implementation

**Optional Topics** (include when relevant):
- **Project Characteristics:** Tech stack, size, conventions, patterns (skip if already clear from assessment)
- **Team Characteristics:** Team size, experience levels, collaboration patterns (skip for solo projects)
- **Pain Points:** Current friction with AI assistance (skip if no prior AI tool usage)
- **Constraints:** Timeline, budget, compatibility requirements, style preferences (include if applicable)
- **Worktree Setup:** Whether to configure git worktrees for parallel development (include if developer uses multiple branches simultaneously)

**AI-Readiness Dimensions:** Understanding, navigation, behavior knowledge, task execution, skills, constraints, growth capability, maintainability.

**AI Personality Configuration:**

Configures AI personality using simplified Big 5 framework mapped to developer concerns:
- Openness → Creativity vs Pragmatism
- Conscientiousness → Thoroughness vs Speed
- Extraversion → Proactive vs Responsive
- Agreeableness → Collaborative vs Direct
- Neuroticism (inverted) → Cautious vs Confident

Plus communication preferences:
- Verbosity (Concise / Balanced / Detailed)
- Technical Level (Assume expertise / Explain / Teach)
- Formality (Professional / Semi-formal / Casual)

**Protocols:**
- AI Personality traits use techniques from `protocols/identity_and_profile.md`
- Communication preferences use techniques from `protocols/communication_style.md`

**AskUserQuestion Tool Usage:**
For Interaction Mode, AI Personality, and AI-readiness priorities, use AskUserQuestion tool to:
- Present options clearly
- Include recommendations with rationale
- Allow "use defaults" option
- Respect developer's time (don't over-interview)

**task.md Structure:** See "Create Task Documentation" action. Must include:
- Project Assessment (tech stack, conventions, current state, gaps)
- Interaction Mode (Lead/Senior/Peer/Junior)
- AI Personality profile (Big 5 traits + communication preferences)
- AI-Readiness Criteria (definition and priorities)
- Implementation Plan (environment setup and skills setup tasks)
- Constraints and Preferences
- Protocols referenced (identity_and_profile.md, communication_style.md)

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Over-interviewing | Developer feels interrogated or loses patience | Infer from context before asking; limit to 3 follow-ups per topic |
| 2 | Wrong Interaction Mode | Mode selected doesn't match developer's actual preference | Explain each mode clearly; allow changes mid-workflow |
| 3 | Incomplete trace on interruption | Session ends mid-action without recoverable state | Write trace events at step boundaries, not just completion |
| 4 | Defaults don't fit project | Applied defaults conflict with project conventions | Document defaults used; allow override in task.md |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
