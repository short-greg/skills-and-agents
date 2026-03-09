---
name: setup-interview
description: >
  Consultative workflow to understand a project and plan AI-readiness setup.
  Interviews developer, establishes interaction mode, defines criteria, outputs implementation plan.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "set up AI environment", "make project AI-ready", "setup project".
argument-hint: "[project path or context]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, WebFetch, AskUserQuestion
---

# Setup Interview

**Goal:** Understand project and team needs, establish interaction mode, define AI-readiness criteria, output approved implementation plan.

**Intent:** Projects differ in structure, conventions, and needs. Before implementing anything, understand what the developer wants and how they want to collaborate with AI.

**Scope:** Consultation from assessment through plan approval. No implementation — that's handled by setup-environment and setup-skills workflows.

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
- setup-environment and setup-skills workflows unlocked (moved to `.claude/skills/`)

**Failure:** Trace documents what was attempted and blockers; partial deliverables noted

---

## Steps

1. Create preliminary checklist → KR1-4. You MUST use TodoWrite with formula: `interview - KR# - <task> - <details>`
2. Check for existing trace → Execute "Recover from Interruption" task if trace exists
2a. Create interview plan → KR1, KR2, KR3, KR4. Execute "Create Interview Plan" task. Output plan for developer approval
3. Assess and gather requirements → KR1, KR2. Execute "Assess and Gather Requirements" task. You MUST establish Interaction Mode and AI Personality during this step
4. Define AI-readiness criteria → KR3. Execute "Define AI-Readiness Criteria" task
5. Plan implementation → KR4. Execute "Plan Implementation" task. Output action plan for approval
6. Create task directory and task.md → KR4. Execute "Create Task Documentation" task. Create `tasks/{TASK_ID}/` directory if it doesn't exist, then write all decisions to `tasks/{TASK_ID}/task.md`. Output confirmation message with file path
7. Unlock next workflows → Move setup-environment and setup-skills to `.claude/skills/`
8. Complete and hand off → Execute "Complete and Hand Off" task. Output completion summary with checklist

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

**Mode:** [orienting](../modes/orienting.md)

**Instructions:**
1. Review the trace
2. Recover your position

**Inputs:**
- Trace file (required)

**Outputs:**
- Recovery summary and confirmed resume point


---

### Plan Implementation (→ KR4)

**Goal:** Create roadmap for achieving AI-readiness

**When:** Definition complete

**Mode:** [planning](../modes/planning.md)

**Instructions:**
1. Propose implementation plan based on definition and Interaction Mode
2. Plan should specify which tasks from setup-environment and setup-skills to execute
3. Get approval before finalizing

**Inputs:**
- Definition document (required)
- Interaction Mode (required)
- Constraints (optional)

**Outputs:**
- Approved implementation plan specifying:
  - Environment setup tasks (for setup-environment workflow)
  - Skills setup tasks (for setup-skills workflow)

---

### Create Interview Plan (→ KR1, KR2, KR3, KR4)

**Goal:** Design interview approach covering all critical topics for this project

**When:** Starting consultation (after checking for existing trace)

**Mode:** [planning](../modes/planning.md)

**Instructions:**
1. Read existing project documentation (README, CLAUDE.md if exists, package.json/requirements.txt, etc.)
2. Based on what you discover, determine which topics are relevant from the **possible topics** below
3. Create interview plan covering the relevant topics (skip irrelevant ones):
   - **Project characteristics:** Tech stack, size, conventions, existing patterns (if not already clear)
   - **Team characteristics:** Team size, experience levels, collaboration patterns (skip for solo projects)
   - **Pain points:** Current friction with AI assistance (if developer has used AI tools)
   - **Constraints:** Timeline, budget, compatibility requirements, style preferences (if applicable)
   - **Success criteria:** How to measure AI-readiness (always include)
   - **Interaction Mode:** How developer wants to collaborate with AI (always include)
   - **AI Personality:** Developer's preferred AI characteristics (always include - see Additional Notes)
   - **Communication Style:** Formality, verbosity, technical level, tone (always include)
4. For each included topic:
   - Why this matters for AI-readiness
   - Sample questions or question templates
   - What to look for in responses
5. Output plan as structured document with explicit relevance determination:
   - **Relevance Assessment:** For each topic, explicitly state whether it's included/excluded and why
   - **Decision Criteria Used:** What signals determined relevance (e.g., "Solo project detected from git log → skipping Team Characteristics", "README already documents stack thoroughly → skipping Project Characteristics", "No previous AI tool usage mentioned → including Pain Points to discover constraints")
   - **Topics Included:** List with specific rationale for each
   - **Topics Excluded:** List with specific rationale for each
6. Ask developer: "Does this interview plan cover what you need? Anything to add or skip?"

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Outputs:**
- Interview plan document with topics, rationale, and question templates
- Developer confirmation of plan

---

### Assess and Gather Requirements (→ KR1, KR2)

**Goal:** Understand project state, developer needs, and establish how developer wants to work with AI

**When:** Starting consultation

**Mode:** [interviewing](../modes/interviewing.md)

**Instructions:**
1. Output Position Statement as consultant and summary of context (read docs, analyze structure, detect patterns)
2. Output assessment summary
3. Ask developer: "Does this match your understanding?" (wait for confirmation)
4. Establish Interaction Mode using AskUserQuestion tool (see Interaction Modes below)
5. Configure AI Personality using AskUserQuestion tool (see Additional Notes: AI Personality Configuration for question structure)
6. Configure Communication Style using AskUserQuestion tool (see Additional Notes: AI Personality Configuration)
7. Ask targeted questions to fill remaining gaps based on interview plan
8. Document all decisions for task.md

**Inputs:**
- Repository access (required)
- Existing documentation (optional)

**Outputs:**
- Interaction Mode (Lead/Senior/Peer/Junior) documented
- AI Personality profile documented (Big 5 traits, communication preferences)
- Assessment document with current state and gaps (confirmed by developer)
- Requirements summary with developer confirmation

**Interaction Modes:**
- **Lead:** AI almost entirely autonomous, developer expects good output without much review
- **Senior:** AI fairly autonomous, receives some feedback, less collaboration than Peer
- **Peer:** AI and developer collaborate heavily on designs and plans
- **Junior:** Developer heavily reviews AI output, confirms designs/plans, AI does not jump to implementation

[COMMENT: Combine with the above]
## Interaction Mode

**Mode:** [Lead/Senior/Peer/Junior]

**Meaning:**
[1-2 sentences explaining what this mode means for AI behavior in this project]

## AI Personality Profile

**Personality Traits:**
- Creativity vs Pragmatism: [Choice with brief note]
- Thoroughness vs Speed: [Choice with brief note]
- Proactive vs Responsive: [Choice with brief note]
- Collaborative vs Direct: [Choice with brief note]
- Cautious vs Confident: [Choice with brief note]

**Communication Preferences:**
- Verbosity: [Concise/Balanced/Detailed]
- Technical Level: [Assume expertise/Explain/Teach]
- Formality: [Professional/Semi-formal/Casual]
- Emoji Usage: [Never/Sparingly/Frequently] (if specified)

**Protocols Referenced:**
- identity_and_profile.md (Personality Traits technique)
- communication_style.md (Verbosity Calibration, Technical Level Adjustment, Formality Selection)

---

### Define AI-Readiness Criteria (→ KR3)

**Goal:** Establish what "AI-ready" means for this project

**When:** Assessment complete, Interaction Mode established

**Mode:** [defining](../modes/defining.md)

**Instructions:**
1. Present AI-readiness dimensions as options (see Additional Notes)
2. Recommend priorities based on assessment
3. Let developer choose focus areas
4. Document agreed criteria

**Inputs:**
- Assessment document (required)
- Interaction Mode (required)

**Outputs:**
- Definition document with agreed criteria


---

### Create Task Documentation (→ KR4)

**Goal:** Document all decisions in structured task.md file

**When:** Implementation plan approved

**Mode:** [implementing](../modes/implementing.md)

**Instructions:**
1. Create `tasks/{TASK_ID}/` directory if it doesn't exist
2. Write `tasks/{TASK_ID}/task.md` with all decisions using the structure below
3. Output confirmation: "Created task.md at tasks/{TASK_ID}/task.md"

**Inputs:**
- Interaction Mode (required)
- AI Personality profile (required)
- Assessment document (required)
- AI-readiness criteria (required)
- Implementation plan (required)
- Constraints and preferences (required)

**Outputs:**
- task.md file created at `tasks/{TASK_ID}/task.md`
- Confirmation message with file path

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

**Environment Setup (for setup-environment workflow):**
- [ ] [Task 1]
- [ ] [Task 2]
...

**Skills Setup (for setup-skills workflow):**
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

### Complete and Hand Off (→ All KRs)

**Goal:** Confirm successful completion and guide developer to next step

**When:** task.md created, workflows unlocked

**Mode:** [positioning](../modes/positioning.md)

**Instructions:**
Output completion summary using this exact format:

```
═══════════════════════════════════════════════════════
SETUP INTERVIEW COMPLETE
═══════════════════════════════════════════════════════

✅ Completion Checklist:

1. task.md created: tasks/{TASK_ID}/task.md
2. Interaction Mode: {Mode}
3. AI Personality: {Brief 1-line summary}
4. Communication Style: {Brief 1-line summary}
5. Project assessment: Documented with {X} gaps identified
6. AI-readiness criteria: Defined and agreed
7. Implementation plan: Approved ({X} environment tasks, {Y} skills tasks)
8. Workflows unlocked:
   - setup-environment → .claude/skills/setup-environment.md
   - setup-skills → .claude/skills/setup-skills.md

═══════════════════════════════════════════════════════
NEXT STEP
═══════════════════════════════════════════════════════

Run: /setup-environment tasks/{TASK_ID}/task.md

This will:
- Create project folders per plan
- Generate CLAUDE.md with your Interaction Mode and AI Personality
- Set up worktree environment (if included in plan)

After setup-environment completes, you'll run /setup-skills to create
your skill builder and initial skills.

═══════════════════════════════════════════════════════
```

**Inputs:**
- task.md path (required)
- All documented decisions (required)

**Outputs:**
- Formatted completion summary
- Clear next step instruction

---

## Additional Notes and Terms

**AI-Readiness Dimensions:** Understanding, navigation, behavior knowledge, task execution, skills, constraints, growth capability, maintainability.

**Customization Levels:**
- Minimal: Basic CLAUDE.md with project overview
- Moderate: CLAUDE.md plus project patterns and conventions
- Comprehensive: Full skill suite with skill builder and custom workflows

**Task Folder:** Recommended: `./tasks/{TASK_ID}/` with `./tasks/{TASK_ID}/local_context/` for task-specific context.

**Docs Folder:** Recommended: `./docs/`

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

**task.md Contents:** Must include:
- Interaction Mode selected (Lead/Senior/Peer/Junior)
- AI Personality profile (Big 5 traits + communication preferences)
- Assessment summary (confirmed by developer)
- AI-readiness criteria agreed
- Implementation plan (with tasks for setup-environment and setup-skills)
- Constraints and preferences discovered
- Protocols referenced (identity_and_profile.md, communication_style.md)

See "Create Task Documentation" task for complete structure.

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Prompting Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
