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
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch
---

# Setup Env

**Goal:** Configure AI-ready development environment per decisions from setup-init.

**Intent:** Preferences were established during setup-init. This workflow implements those decisions while respecting Interaction Mode for HOW to execute.

**Scope:** Create project structure (folders, CLAUDE.md with full AI profile, worktrees). No coding conventions (requires tech stack decisions). No skill creation — that's setup-skill-builder.

**Workflow Style:** Goal-Oriented (Declarative). Achieve outcomes; adapt approach per Interaction Mode.

---

## Key Results - KR

1. Project folders created per plan (docs, tasks, etc.)
2. CLAUDE.md created with full AI profile (Interaction Mode, AI Personality, Communication Style)
3. Worktree environment configured (if plan includes it)

## Requirements and Constraints - REQ

1. Read task.md first — do not proceed without it
2. Research current best practices before proposing (use WebSearch)
3. Respect Interaction Mode throughout (see Additional Notes for behavior per mode)
4. Output all proposals as code blocks
5. Do not re-interview WHAT to do — but collaborate on HOW per Interaction Mode
6. Track progress per `tracking_and_recovery.md`

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
6. Evaluate results → Execute A5
7. Install setup-skill-builder → If successful, copy `workflows/setup/setup-skill-builder.md` to `.claude/skills/setup-skill-builder.md`

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

### A3: Design Environment (→ KR1, KR2, KR3)

Intent: Create environment proposal per Interaction Mode
KR: Environment proposal approved by developer
Preconditions:
- Required: Oriented findings
- Required: task.md decisions
Postconditions:
- Success: Output environment proposal as code block, output approval confirmation
- Failure: Output proposal, output feedback captured for iteration
Exit Conditions:
- Developer rejects proposal 3 times → stop, escalate
- Oriented findings incomplete → stop, complete A2 first

Objectives (OBJ):
1. Generate environment proposal (folders, CLAUDE.md structure, worktrees if applicable)
2. Collaborate per Interaction Mode (see Additional Notes)
3. Present proposal as code block for review

Constraints (CONST):
1. Read `thinking.md` and apply Alternative Generation — to explore options
2. Read `pragmatics.md` and apply Option Presentation — to structure proposal
3. Respect Interaction Mode collaboration style
4. Output proposal as code block

Validation:
1. Add OBJ1-3 to TodoWrite checklist
2. Output validation: list each OBJ with evidence it was achieved

---

### A4: Implement Environment (→ KR1, KR2, KR3)

Intent: Create folders, CLAUDE.md, and worktrees per approved design
KR: Environment artifacts created and verified
Preconditions:
- Required: Approved design
- Required: task.md with full AI profile
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
4. Write CLAUDE.md with Interaction Mode, AI Personality, Communication Style sections
5. Configure worktrees if in plan (follow worktree-setup-guideline.md)
6. Confirm each artifact created successfully
7. Output implementation summary

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
3. Confirm CLAUDE.md contains full AI profile (Interaction Mode, AI Personality, Communication Style)
4. Confirm folders exist per plan
5. Confirm worktree configuration if applicable
6. Output verification results with evidence
7. If successful, output: "Environment ready. Run `/setup-skill-builder` to continue."

---

## Additional Notes and Terms

**Interaction Mode Behavior:**
- **Lead:** Research, propose, execute. Minimal discussion.
- **Senior:** Research, propose, get confirmation before executing.
- **Peer:** Research, present multiple options, debate alternatives, iterate on design together.
- **Junior:** Research, explain each option, get explicit guidance on which to pursue.

**CLAUDE.md Full AI Profile:**
Must include all of these sections:
```markdown
## Interaction Mode

This project uses **[Mode]** collaboration style:
- [Description of what this means for AI behavior]

## AI Personality

- Creativity vs Pragmatism: [Choice]
- Thoroughness vs Speed: [Choice]
- Proactive vs Responsive: [Choice]
- Collaborative vs Direct: [Choice]
- Cautious vs Confident: [Choice]

## Communication Style

- Verbosity: [Concise/Balanced/Detailed]
- Technical Level: [Assume expertise/Explain/Teach]
- Formality: [Professional/Semi-formal/Casual]
```

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Overwriting existing files | CLAUDE.md or folders already exist | Confirm before overwriting; back up existing files |
| 2 | Research outdated | Best practices change faster than workflow updates | Use WebSearch for current patterns; cite sources with dates |
| 3 | Worktree misconfiguration | Git worktrees set up incorrectly | Follow worktree-setup-guideline.md exactly; verify with git status |
| 4 | task.md drift | Implementation diverges from task.md decisions | Reference task.md at each step; update if changes approved |

---

## References

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [worktree-setup-guideline.md](../../guidelines/worktree-setup-guideline.md)
