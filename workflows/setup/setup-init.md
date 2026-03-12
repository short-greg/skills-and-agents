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
5. Iterate up to 3 times per task if developer requests changes before escalating
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

1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with formula: `interview - KR# - <task> - <details>`
2. Check for existing trace → Execute "Recover from Interruption" task if trace exists
3. Assess project → KR2. Execute "Assess Project" task. Output assessment summary
4. Plan interview → KR1-4. Execute "Plan Interview" task. Output interview plan for approval
5. Conduct interview → KR1, KR2, KR3. Execute "Conduct Interview" task
6. Create task.md → KR3, KR4. Execute "Create Task Documentation" task. Document all decisions
7. Install setup-env → Copy `workflows/setup/setup-env.md` to `.claude/skills/setup-env.md`
8. Output completion summary → Inform user: "Setup init complete. Run `/setup-env tasks/{TASK_ID}/task.md` to continue."

---

## Tasks

**REQUIRED:**
1. Each task specifies modes to use.
2. When executing a task you MUST read those modes if you haven't already.
3. When executing a Task:
   1. You MUST Read the mode to understand typical actions
   2. You MUST choose actions based on the Instructions and the Mode.

### Recover from Interruption (→ KR1, KR2, KR3, KR4)

**Goal:** Resume consultation with full context

**When:** Trace file exists from previous session

**Mode:** [orienting](../../modes/orienting.md)

**Instructions:**
1. Review the trace
2. Recover your position

**Inputs:**
- Trace file (required)

**Outputs:**
- Recovery summary and confirmed resume point


---

### Assess Project (→ KR2)

**Goal:** Understand project state and identify common considerations

**When:** Starting consultation (after checking for existing trace)

**Mode:** [orienting](../../modes/orienting.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Focus on AI-readiness considerations
- Output assessment summary when complete

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Outputs:**
- Assessment summary with findings

---

### Plan Interview (→ KR1, KR2, KR3, KR4)

**Goal:** Determine relevant topics and create interview approach

**When:** After project assessment

**Mode:** [planning](../../modes/planning.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- See possible topics in Additional Notes
- For each included topic, explain why it matters
- Output interview plan for developer confirmation

**Inputs:**
- Assessment summary (required)

**Outputs:**
- Interview plan with topics to cover and rationale

---

### Conduct Interview (→ KR1, KR2, KR3)

**Goal:** Interview developer according to plan

**When:** After interview plan approved

**Mode:** [interviewing](../../modes/interviewing.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Follow interview plan topics
- Infer from context before asking
- Document all decisions

**Inputs:**
- Interview plan (required)

**Outputs:**
- Interaction Mode (Lead/Senior/Peer/Junior)
- AI Personality profile (Big 5 traits, communication preferences)
- AI-readiness criteria and priorities
- Constraints and preferences
- All interview responses documented

---

### Create Task Documentation (→ KR3, KR4)

**Goal:** Define AI-readiness criteria, compose implementation plan, document all decisions

**When:** Interview complete

**Mode:** [defining](../../modes/defining.md)

**Instructions:**
Read all actions in the mode and choose appropriate actions to achieve the goal.

Additional guidance:
- Define what "AI-ready" means for this project
- Create `tasks/{TASK_ID}/` directory and write task.md
- Output confirmation with file path

**Inputs:**
- Interview responses (required)
- Interaction Mode, AI Personality, constraints (required)

**Outputs:**
- AI-readiness definition
- Implementation plan
- task.md file at `tasks/{TASK_ID}/task.md`

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

**task.md Structure:** See "Create Task Documentation" task. Must include:
- Project Assessment (tech stack, conventions, current state, gaps)
- Interaction Mode (Lead/Senior/Peer/Junior)
- AI Personality profile (Big 5 traits + communication preferences)
- AI-Readiness Criteria (definition and priorities)
- Implementation Plan (environment setup and skills setup tasks)
- Constraints and Preferences
- Protocols referenced (identity_and_profile.md, communication_style.md)

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
