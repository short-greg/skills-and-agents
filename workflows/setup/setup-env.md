---
name: setup-env
description: >
  Configure development environment for AI-readiness based on setup-init decisions.
  Creates folders, CLAUDE.md (with full profile), worktrees. Respects Interaction Mode throughout.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this workflow.
  Triggers on: "setup environment", "setup env", "create CLAUDE.md", "set up project structure".
argument-hint: "[task.md path]"
disable-model-invocation: true
user-invocable: true
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion
---

# Setup Env

**Goal:** Configure AI-ready development environment per decisions from setup-init.

**Intent:** Preferences were established during setup-init. This workflow implements those decisions while respecting Interaction Mode for HOW to execute.

**Scope:** Create project structure (folders, CLAUDE.md with full AI profile, worktrees), plus continuous improvement infrastructure (CLAUDE.md maintenance system, skill feedback tracking). No coding conventions (requires tech stack decisions). No skill creation — that's setup-skill-builder.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach per Interaction Mode.

---

## Key Results - KR

1. Project folders created per plan (docs, tasks, etc.)
2. CLAUDE.md created with full AI profile (Interaction Mode, AI Personality, Communication Style)
3. Worktree environment configured (if plan includes it)
4. Continuous improvement infrastructure established (CLAUDE.md maintenance, skill feedback tracking)

## Requirements and Constraints - REQ

1. Read task.md first — do not proceed without it
2. Research current best practices before proposing (use WebSearch)
3. Respect Interaction Mode throughout (see Additional Notes for behavior per mode)
4. Output all proposals as structured markdown documents
5. Do not re-interview WHAT to do — but collaborate on HOW per Interaction Mode
6. Track progress per `tracking_and_recovery.md`
7. You MUST get explicit approval before implementing — output proposal summary, ask "Does this look correct?", wait for confirmation
8. Echo back what you understood from task.md before proposing — confirm Interaction Mode and AI Personality are correct

## Preconditions

**Required:**
- task.md from setup-init at `tasks/{TASK_ID}/task.md`
- Implementation plan specifying what to set up

**Optional:**
- Existing project structure to preserve

## Postconditions

**Success:**
- Project folders created per plan
- CLAUDE.md files written with full AI profile
- Worktree configuration complete (if specified)
- setup-skill-builder installed to `.claude/skills/`

**Failure:** Trace documents what was attempted and blockers

---

## Steps

0. Check preconditions → Verify task.md exists. **If not, STOP:** "Run `/setup-init` first to create task.md."
1. Read task.md → Understand decisions and Interaction Mode
2. Investigate best practices → KR1, KR2. Execute A1
3. Orient to findings → KR1, KR2, KR3. Execute A2
4. Design environment → KR1, KR2, KR3. Execute A3. Output proposal as code block
5. Implement environment → KR1, KR2, KR3. Execute A4
6. Set up continuous improvement → KR4. Execute A6
7. Evaluate results → Execute A5
8. Install setup-skill-builder → If successful, copy `workflows/setup/setup-skill-builder.md` to `.claude/skills/setup-skill-builder.md`

---

## Actions

Actions are units of work that apply protocol techniques to achieve specific outcomes.

**Precondition:** setup-init must have been executed.

### A1: Investigate Best Practices (→ KR1, KR2)

Intent: Research current AI-readiness environment patterns
KR: Research findings on folder structures, CLAUDE.md patterns, and worktree practices documented
Preconditions:
- Required: task.md context
Postconditions:
- Success: Output research findings with sources cited
- Failure: Output partial findings with gaps noted
Exit Conditions:
- WebSearch unavailable → stop, use existing knowledge and note limitation

Instructions:
1. Read `thinking.md` and apply Evidence-Based Reasoning — to evaluate sources
2. Read `discipline.md` and apply MECE Enumeration — to structure research topics
3. Use WebSearch for: folder structures, CLAUDE.md best practices, worktree patterns
4. Document findings with sources
5. Output research summary

---

### A2: Orient to Findings (→ KR1, KR2, KR3)

Intent: Understand research in context of project needs
KR: Contextualized understanding of applicable patterns documented
Preconditions:
- Required: Research findings
- Required: task.md
Postconditions:
- Success: Output patterns matched to project requirements with gaps/conflicts noted
- Failure: Output incomplete analysis with blockers identified
Exit Conditions:
- Research findings empty → stop, complete A1 first
- task.md inaccessible → stop, verify preconditions

Objectives (OBJ):
1. Compare research findings against task.md requirements
2. Identify which patterns apply to this project
3. Note gaps where research doesn't address project needs
4. Note conflicts where patterns contradict project constraints

Constraints (CONST):
1. Read `pragmatics.md` and apply Context Assessment — to evaluate applicability
2. Prioritize project requirements over general best practices

Validation:
1. Add OBJ1-4 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A3: Design Environment (→ KR1, KR2, KR3, KR4)

Intent: Create comprehensive environment proposal per Interaction Mode
KR: Environment proposal approved by developer
Preconditions:
- Required: Oriented findings
- Required: task.md decisions with Interaction Mode, AI Personality, Communication Style
Postconditions:
- Success: Output environment proposal as structured document, output explicit approval confirmation
- Failure: Output proposal, output feedback captured for iteration
Exit Conditions:
- Developer rejects proposal 3 times → stop, escalate
- Oriented findings incomplete → stop, complete A2 first

Instructions:
1. Read `thinking.md` and apply Alternative Generation — to explore options
2. Echo back task.md understanding: "Based on task.md, I understand your preferences are: [Interaction Mode], [AI Personality traits], [Communication Style]. Is this correct?"
3. Wait for confirmation before proceeding
4. Use AskUserQuestion for these decisions (unless task.md already specifies):
   - Folder structure approach (if multiple valid options)
   - CLAUDE.md detail level (concise vs comprehensive)
   - Tooling integrations (MCP servers, hooks) — ask what the developer wants
   - Migration approach for existing files (if any)
5. Read `pragmatics.md` and apply Option Presentation — to structure proposal
6. Generate proposal covering ALL sections in Proposal Template (see Additional Notes)
7. Output complete proposal as structured markdown
8. Ask: "Does this proposal look correct? Any changes before I implement?"
9. Wait for explicit approval — do NOT proceed to A4 until developer confirms

---

### A4: Implement Environment (→ KR1, KR2, KR3)

Intent: Create folders, CLAUDE.md, and worktrees per approved design
KR: Environment artifacts created and verified
Preconditions:
- Required: Approved design (explicit confirmation from A3)
- Required: task.md with Interaction Mode, AI Personality, Communication Style
Postconditions:
- Success: Output folders created, output CLAUDE.md written, output worktree config if applicable
- Failure: Output partial implementation, output errors documented
Exit Conditions:
- Design not approved → stop, complete A3 first
- Filesystem error → stop, report error and request guidance

Instructions:
1. Read `discipline.md` and apply Sequential Processing — to implement in order
2. Read `tracking_and_recovery.md` and apply Dual Tracking — to record progress
3. Create folders per design
4. Write CLAUDE.md — translate AI Personality from task.md into actionable instructions:

   **Interaction Mode → Behavioral instructions:**
   - Lead: "Proceed autonomously. Inform after completion. Minimal confirmation needed."
   - Senior: "Proceed with brief confirmation. Request feedback on major decisions only."
   - Peer: "Present options with trade-offs. Confirm approach before implementing."
   - Junior: "Explain reasoning thoroughly. Wait for explicit approval. Never assume."

   **Personality traits → CLAUDE.md instructions:**
   - High Creativity: "Suggest creative alternatives. Explore unconventional solutions."
   - High Pragmatism: "Prefer proven patterns. Minimize novelty and experimentation."
   - High Thoroughness: "Provide comprehensive analysis. Cover edge cases thoroughly."
   - High Speed: "Be concise. Prioritize progress over completeness."
   - Proactive: "Anticipate needs. Suggest improvements proactively."
   - Responsive: "Wait for explicit requests. Don't suggest unless asked."
   - Collaborative: "Share reasoning. Invite feedback. Iterate together."
   - Direct: "State conclusions directly. Minimize unnecessary explanation."
   - Cautious: "Verify assumptions before acting. Confirm risky operations."
   - Confident: "Proceed decisively. Minimal hedging."

   **Communication → CLAUDE.md instructions:**
   - Concise: "Keep responses brief. Use bullet points over paragraphs."
   - Detailed: "Explain thoroughly. Include context and rationale."
   - Assume expertise: "Use technical terms without explanation."
   - Teach: "Explain concepts. Include learning context."

5. Configure worktrees if in plan (follow worktree-setup-guideline.md)
6. Create `.claude/settings.json` if tooling integrations were requested in A3
7. Confirm each artifact created successfully
8. Output implementation summary with list of all files created

---

### A6: Set Up Continuous Improvement (→ KR4)

Intent: Establish infrastructure for keeping AI environment current and learning from experience
KR: CLAUDE.md maintenance system and skill feedback tracking in place
Preconditions:
- Required: CLAUDE.md created
- Required: .claude/skills/ folder exists
Postconditions:
- Success: Output maintenance hooks documented, output feedback template created
- Failure: Output partial setup, output what could not be created
Exit Conditions:
- CLAUDE.md not found → stop, complete A4 first

Instructions:
1. Read `tracking_and_recovery.md` and apply Progress Recording — to design tracking approach
2. Create `.claude/feedback/` folder for skill usage feedback
3. Create `.claude/feedback/README.md` documenting the feedback system
4. Create `.claude/feedback/template.md` for post-skill-use reports
5. Add "Maintenance" section to CLAUDE.md with update triggers
6. Output summary of continuous improvement infrastructure

---

### A5: Evaluate Results (→ All KRs)

Intent: Verify environment created correctly and hand off
KR: All KRs verified with evidence
Preconditions:
- Required: Implementation outputs
Postconditions:
- Success: Output verification results, output hand off message
- Failure: Output verification failures with remediation steps
Exit Conditions:
- Critical verification failure → stop, report and await guidance
- All KRs pass → proceed to hand off

Instructions:
1. Read `software_quality.md` and apply Correctness and Completeness — to assess artifacts
2. Read `criteria_setting.md` and apply Binary Criteria — for pass/fail on each KR
3. Confirm CLAUDE.md contains full AI profile (Interaction Mode, AI Personality, Communication Style, Maintenance)
4. Confirm folders exist per plan
5. Confirm worktree configuration if applicable
6. Confirm `.claude/feedback/` folder exists with README.md and template.md
7. Output verification results with evidence for each KR
8. If successful, output: "Environment ready. Run `/setup-skill-builder` to continue."

---

## Additional Notes and Terms

**Interaction Mode Behavior:**
- **Lead:** Research, propose, execute. Minimal discussion.
- **Senior:** Research, propose, get confirmation before executing.
- **Peer:** Research, present multiple options, debate alternatives, iterate on design together.
- **Junior:** Research, explain each option, get explicit guidance on which to pursue.

**Proposal Template:**
A3 proposals MUST cover all these sections with specific recommendations:

```markdown
# Environment Proposal

## 0. Confirmed Understanding
Based on task.md:
- **Interaction Mode:** [Mode] — [what this means for AI behavior]
- **AI Personality:** [list each trait and its setting]
- **Communication Style:** [verbosity, technical level, formality]

Please confirm this is correct before I continue.

## 1. Folder Structure
[Tree diagram showing proposed structure]
- Rationale for each top-level folder
- What goes where

## 2. CLAUDE.md Structure
[Outline of sections to include]
- AI Profile (Mode, Personality, Communication) — translated into actionable instructions
- Project context
- Conventions and patterns
- Available skills

## 3. Tooling Integrations
[Use AskUserQuestion to ask developer what they want]
- **MCP Servers:** [None / GitHub / Other — based on developer response]
- **Hooks:** [None / Formatting / Validation / Other — based on developer response]
- **settings.json:** [What to include if any integrations requested]

## 4. Worktrees (if applicable)
[Configuration or "Not applicable" with reason]

## 5. Continuous Improvement
- CLAUDE.md maintenance triggers (when to update)
- Skill feedback tracking approach
- Post-mortem template location

## 6. Migration Plan
[For existing files that need moving/updating]
- Files to migrate
- Files to archive
- Files to delete

## 7. Summary
[Brief summary of what will be created]

**Does this proposal look correct? Any changes before I implement?**
```

**CLAUDE.md Full AI Profile:**
Must include all sections with TRANSLATED instructions (not just trait names). Example for a Peer mode, Creative, Thorough developer:

```markdown
## Interaction Mode

This project uses **Peer** collaboration style.

**How to work with me:**
- Present options with trade-offs before implementing
- Confirm approach on significant decisions
- Share your reasoning and invite feedback
- Iterate together on designs

## AI Personality

**Creativity:** Suggest creative alternatives. Explore unconventional solutions when they might be better.

**Thoroughness:** Provide comprehensive analysis. Cover edge cases. Don't skip details.

**Proactive:** Anticipate needs. Suggest improvements proactively. Don't wait to be asked.

**Collaborative:** Share reasoning. Invite feedback. Iterate together.

**Confident:** Proceed decisively on routine matters. Verify assumptions on risky operations.

## Communication Style

**Verbosity:** Balanced — concise but include necessary context.

**Technical Level:** Assume expertise — use technical terms without explanation.

**Formality:** Semi-formal — professional but not stiff.

## Maintenance

Update this file when:
- New conventions are established
- Skills are added or changed
- AI behavior needs adjustment
- Post-mortems reveal patterns

Last updated: [date]
```

**Note:** The actual content MUST be translated from task.md — do not copy placeholders. Each personality trait becomes a behavioral instruction.

**Skill Feedback System:**
The `.claude/feedback/` folder tracks skill usage outcomes:
```markdown
# Skill Feedback Template

**Skill:** [skill name]
**Date:** [date]
**Outcome:** [Success/Partial/Failed]

## What worked
- [observation]

## What didn't work
- [observation]

## Suggested improvements
- [suggestion]

## Should update CLAUDE.md?
- [ ] Yes — [what to add/change]
- [ ] No
```

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Overwriting existing files | CLAUDE.md or folders already exist | Confirm before overwriting; back up existing files |
| 2 | Research outdated | Best practices change faster than workflow updates | Use WebSearch for current patterns; cite sources with dates |
| 3 | Worktree misconfiguration | Git worktrees set up incorrectly | Follow worktree-setup-guideline.md exactly; verify with git status |
| 4 | task.md drift | Implementation diverges from task.md decisions | Reference task.md at each step; update if changes approved |
| 5 | Feedback system unused | Developers don't submit feedback after skill use | Make feedback lightweight; integrate into skill postconditions |
| 6 | CLAUDE.md stale | Maintenance section ignored | Include update triggers in skill workflows; periodic review reminder |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
