# Skill Builder Template

Template for skills created by the skill-builder.

**Notation:**
- `{PLACEHOLDER}` — Configurable values. Replace based on developer preferences.
- `<guidelines>` — Guidance for the skill-builder. Do not include in output.

---

## Consultant Behavior (When Creating Skills)

When creating a skill, the skill-builder must act as a consultant:

**Reason Out Loud:**
- "To create a good [skill type] skill, I need to understand..."
- "Based on the project type, I should consider..."
- "I found [X] in the codebase, which suggests..."

**Assume and Explain Role:**
- After understanding what skill is needed, state your role
- Example: "For this deployment skill, I'll think as a DevOps engineer focused on reliability and rollback safety"
- Explain what this role means for the skill design

**Share Findings Before Asking:**
- Discover what exists (patterns, conventions, similar skills)
- Output what you found and what it implies
- Then ask targeted questions about gaps

**Recommend with Rationale:**
- Don't just ask "what tools do you want?"
- Instead: "This skill will modify files and run tests, so I recommend Edit, Bash, and TodoWrite for tracking. Does that sound right?"

**Research Before Recommending:**
- Use WebSearch to find best practices for the skill's domain
- Search: "[skill topic] best practices", "[domain] workflow patterns"
- Incorporate findings into skill design

---

## When to Create a Skill

<Before creating a skill, verify it's appropriate:>

<Create a skill when:>
<- Task requires 2+ actions in sequence>
<- Task will be repeated (not one-off)>
<- Task benefits from tracking/recovery>
<- Task needs consistent execution>

<Do NOT create a skill when:>
<- Task is single action — execute the action directly>
<- Task is one-off with no reuse value>
<- Task is purely exploratory/research>

---

## Skill Definition

<Minimum requirements for a well-formed skill:>

<| Attribute | Requirement |>
<|-----------|-------------|>
<| Actions | 2+ |>
<| Key Results | As few as needed to be complete |>
<| Recovery | Required for multi-step skills |>
<| Progress tracking | Required (TodoWrite) |>
<| Line count | Target 150-200 lines (200-250 with good reason) |>

---

## Available Claude Code Tools

When creating skills, recommend appropriate tools. Reason out loud about why each is needed:

| Tool | Purpose | Permission | Recommend When |
|------|---------|------------|----------------|
| Read | Read files | No | All skills need this |
| Write | Create/overwrite files | Yes | Skills that create artifacts |
| Edit | Targeted file edits | Yes | Skills that modify existing files |
| Glob | Find files by pattern | No | Search, discovery, refactoring |
| Grep | Search file contents | No | Code search, pattern finding |
| Bash | Shell commands | Yes | Git, builds, tests, CLI tools |
| NotebookEdit | Jupyter cell editing | Yes | Data science, notebook skills |
| TodoWrite | Progress tracking | No | Multi-step skills (non-interactive) |
| Task* | Task management | No | Interactive sessions, background work |
| WebSearch | Web search | Yes | Skills needing current info |
| WebFetch | Fetch URL content | Yes | Documentation, API research |
| AskUserQuestion | User input | No | Consultative, decision-making |
| Agent | Spawn subagents | No | Complex parallel work |
| LSP | Code intelligence | No | Type checking, navigation |
| EnterPlanMode | Design before coding | No | Complex implementation skills |

**Tool Selection Reasoning:**
- "This skill creates files, so it needs Write"
- "This skill searches code, so Glob and Grep"
- "This skill needs user decisions, so AskUserQuestion"
- "This is a multi-step skill, so TodoWrite for tracking"

---

## MCP and Hooks Discovery

Before finalizing a skill, check what's available and recommend additions:

**Discovery:**
1. Check existing configs: `~/.claude/settings.json`, `.claude/settings.json`
2. Check existing hooks: `~/.claude/hooks/`, `.claude/hooks/`
3. Research: Use WebSearch to find MCPs relevant to skill's domain

**Reasoning:**
- "For a [skill purpose] skill, these MCPs would help because..."
- "I found an existing [X] hook that this skill should use"
- "This skill would benefit from a [Y] MCP for [reason]"

**Common MCP categories:**
- Database MCPs (for data access skills)
- API MCPs (for integration skills)
- File system MCPs (for specialized file handling)
- Communication MCPs (Slack, email, etc.)

---

## Consultation Approach by Interaction Mode

| Mode | Approach |
|------|----------|
| Lead | Output proposal with rationale, confirm only essential points, proceed efficiently |
| Senior | Share intermediate findings, recommend approach, get approval at gates |
| Peer | Brainstorm options together, collaborate on design, discuss trade-offs |
| Junior | Present all options with trade-offs, await direction at each decision |

---

## Skill Analysis Questions

Use these to understand intent. Reason out loud about which questions matter for this skill.

**Understanding the Problem:**
- What problem does this skill solve?
- Why is this skill necessary? (vs manual, vs existing skill)
- Who will use this skill? What triggers it?

**Defining Scope:**
- What are the deliverables?
- Should this be one skill or multiple?
- Does it compose with other skills?

**Determining Role:**
- What domain role? (software engineer, designer, analyst, reviewer)
- What behavioral stance? (executor, advisor, pair programmer)

**Deciding Behavior:**
- How autonomous? (proceed vs confirm at each step)
- How handle errors? (fail, retry, ask, rollback)

---

## Variation Dimensions

| Dimension | Options | Determined by |
|-----------|---------|---------------|
| Workflow Style | Declarative, Imperative, Hybrid | Developer preference |
| Action Style | Instructions, Objectives+Constraints | Per-action need |
| Role | Domain + Behavioral | Skill intent |
| Autonomy | Proceed, confirm at gates, confirm each step | Interaction Mode |
| Artifacts | SKILL.md only, +references, +code, +hooks | Skill needs |

---

## Meta Actions

Some actions don't perform work directly — they decide WHICH actions to perform. These are "meta actions."

**What is a meta action?**
A meta action outputs a list of actions the agent will take, rather than executing work itself. It's about planning the approach, not doing the approach.

**Why use meta actions?**
- **Declarative skills need them** — In declarative workflows, the skill doesn't prescribe a fixed sequence. A meta action determines the sequence at runtime based on context.
- **User buy-in** — Before executing, the agent proposes what it will do and gets approval.
- **Adaptability** — Different situations require different action combinations.

**When to use meta actions:**
- Consultative skills where approach varies by problem
- Skills with many optional actions
- Skills where user should approve the plan before execution

**Example: A4: Propose Actions (from /consult)**

```markdown
### A4: Propose Actions (→ KR2)

Intent: Identify which actions the agent will execute to meet the user's request
KR: Best actions identified and approved by user

Objective: To output a list of the best actions to meet the user's request

Constraints:
1. You MUST follow the action template in the actions you output
2. You MUST start by outputting the actions an expert would often take for this type of problem
3. You MUST select the subset of actions that you recommend
4. The actions MUST help fulfill the objective and key results
5. You MUST follow the appropriate interaction level (Lead: transparently report and proceed; Senior: get confirmation; Peer: collaborate on approach; Junior: present variety of suggestions, await direction)
6. You MUST present proposed actions using the method appropriate to the interaction level (Lead: state actions and proceed; Senior: present actions and ask for confirmation; Peer: present actions and collaborate on refinements; Junior: present variety of options and await direction)
```

**Key distinction:**
- Regular action: "Write the code" (does work)
- Meta action: "Identify which actions to take, then get approval" (plans work)

---

## Skill Validation Checklist

**Structure:**
- [ ] All sections present (Goal, Intent, Scope, Role, KRs, REQs, Pre/Postconditions, Steps, Actions, Notes, Risks, References)
- [ ] Key Results — as few as needed to be complete
- [ ] 2+ Actions with complete structure
- [ ] Target 150-200 lines (200-250 with good reason)

**Content:**
- [ ] Coherent — no contradictions between sections
- [ ] Complete — all KRs achievable from Actions
- [ ] Concise — no duplication, minimum words
- [ ] Precise — unambiguous language, no hedging
- [ ] Referenced protocols exist in `protocols/`
- [ ] Tools in allowed-tools are available

**Semantic (content matches definitions):**
- [ ] Objectives are outcomes — statements of what success looks like, NOT imperative steps
- [ ] Constraints use "You MUST" format — rules to follow, NOT outcomes
- [ ] Instructions are imperative steps — numbered actions to take, NOT outcomes
- [ ] KRs are measurable — success criteria with observable evidence
- [ ] Preconditions state what must exist — inputs/state, NOT actions to take
- [ ] Postconditions state resulting state — Success/Failure outcomes, NOT actions
- [ ] Exit Conditions use format: `{condition} → stop, {action}`
- [ ] Protocol references use format: "Read X and apply Y"

**Quality:**
- [ ] Recovery included — progress tracking, recovery from interruption (if multi-step)
- [ ] Research completed — References section has sources consulted
- [ ] Actions are small — one clear goal each, predictable inputs → outputs

---

## Required Initial Skills

When setting up a project, always create a `/consult` skill for flexible consultative tasks.

See [consult-template.md](consult-template.md) for the full template.

---

## Risk Analysis (Before Creating Skill)

Before finalizing any skill, the skill-builder must perform risk analysis and consult the developer:

**Identify risks in these categories:**
1. **Scope risks** — Is the skill too broad? Too narrow? Overlapping with existing skills?
2. **Execution risks** — What could go wrong during skill execution? Data loss? Incorrect outputs?
3. **Integration risks** — How might this skill conflict with other skills or project conventions?
4. **Autonomy risks** — Is the skill too autonomous for its impact? Should it confirm more?
5. **Artifact risks** — Could generated files overwrite important content? Create clutter?

**Output risk analysis:**
- List each identified risk with severity (high/medium/low)
- Propose mitigations for high/medium risks
- Ask developer to accept, modify, or add mitigations
- Incorporate accepted mitigations into skill's Risks section

---

## Design Proposal (Before Creating Draft)

Before creating a full skill draft, propose the high-level design for approval:

**Output design proposal:**
1. Goal, Intent, Scope, Role — one sentence each
2. Proposed KRs (3-4) — measurable outcomes
3. Proposed Actions — outline with names and one-line purposes
4. Workflow style recommendation — with rationale for why this style fits
5. Anticipated risks — high-level, to be detailed later

**Get approval before proceeding to draft.** This prevents wasted effort on wrong direction.

---

## Iterative Improvement

Skills rarely perfect on first draft. The skill-builder should:

**After creating initial draft:**
1. Output the draft skill for developer review
2. Ask: "What would you change about this skill?"
3. Iterate based on feedback (up to 3 iterations before escalating)

**During iteration, check:**
- Does the skill match the developer's mental model?
- Are the actions at the right granularity?
- Is the workflow style appropriate for how the developer works?
- Are risks adequately addressed?

**After final approval:**
- Validate against Skill Validation Checklist (above)
- Output evidence for each validation criterion

---

## Skill Template

See [skill-template.md](skill-template.md) for the structure all skills must follow.
