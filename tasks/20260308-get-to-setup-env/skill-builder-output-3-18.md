---
name: skill-builder
description: >
  Create new skills following project conventions through consultative process.
  Triggers on: "/skill-builder", "create a skill", "new skill".
argument-hint: "[skill-name] - optional name for the skill to create"
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Grep, TodoWrite, AskUserQuestion
---

# Skill Builder

**Goal:** Create new skills following project conventions through consultative process.

**Intent:** Skills need consistent structure and should align with CLAUDE.md conventions. Without a builder, skills may be inconsistent, miss important sections, or fail to integrate with existing patterns.

**Scope:** Covers skill creation from intent discovery through validation. Does NOT cover skill maintenance, deprecation, or bulk skill updates.

**Role:** Software engineer / Pair programmer — works collaboratively with developer to design skills. Proposes, gets feedback, iterates.

**Workflow Style:** Hybrid — fixed context gathering and validation, flexible design and iteration.

---

## Key Results - KR

1. Skill intent and scope clearly defined before drafting
2. Skill created with complete structure per template
3. Risks identified and mitigations documented
4. Skill validated against checklist with evidence

## Requirements and Constraints - REQ

1. Read CLAUDE.md first — understand conventions before creating
2. Follow Interaction Mode from CLAUDE.md (currently: Peer)
3. Progress tracked per `tracking_and_recovery.md`
4. Propose design before drafting — get approval at gate
5. Iterate up to 3 times before escalating
6. Validate with positive AND negative evidence

---

## Preconditions

**Required:**
- CLAUDE.md with project conventions
- Clear intent for what skill should do (from user or discovered through A2)

**Optional:**
- Existing skills to reference for patterns
- Specific protocols the skill should use

## Postconditions

**Success:**
- Skill file created at `.claude/skills/{skill-name}/SKILL.md`
- CLAUDE.md updated with new skill in Available Skills table
- Validation evidence documented

**Failure:**
- Partial progress documented in trace
- Blockers identified for user resolution

---

## Steps

1. Create checklist → KR1-4. Use TodoWrite with formula: `skill-builder - KR# - <action>`
2. Gather context → Execute A1
3. Analyze intent → Execute A2 (skip if user provided clear intent)
4. Propose design → Execute A3, get approval before continuing
5. Create draft → Execute A4
6. Analyze risks → Execute A5
7. Iterate → Execute A6 (up to 3 rounds based on feedback)
8. Validate → Execute A7
9. Update CLAUDE.md → Add skill to Available Skills table

---

## Actions

### A1: Gather Context (→ KR1, KR2)

Intent: Understand project conventions and existing patterns before creating skill
KR: Context gathered, conventions understood
Preconditions:
- Required: CLAUDE.md exists
Postconditions:
- Success: Output summary of relevant conventions, existing skills, applicable protocols
- Failure: Output which context files missing
Exit Conditions:
- CLAUDE.md missing → stop, request file creation first

Instructions:
1. Read CLAUDE.md — note Interaction Mode, conventions, existing skills
2. Read existing skills in `.claude/skills/` — note patterns and structure
3. Identify protocols relevant to new skill's domain
4. Output context summary for reference during creation

---

### A2: Analyze Skill Intent (→ KR1)

Intent: Discover what problem the skill solves and define its scope
KR: Clear intent, scope, and role documented
Preconditions:
- Required: Context from A1
- Optional: User-provided skill description
Postconditions:
- Success: Output documented intent, scope, deliverables, role
- Failure: Output what could not be determined, request clarification
Exit Conditions:
- User cannot articulate problem → stop, suggest they return when clearer
- Skill overlaps significantly with existing → stop, propose extending existing skill

Instructions:
1. Read `elicitation.md` and apply Inference First — infer what you can from context
2. Ask skill analysis questions (see Notes) until intent is clear
3. Apply Question Pacing — 1-2 questions at a time
4. Output understanding summary for confirmation:
   - Problem solved
   - Scope (what's in, what's out)
   - Deliverables
   - Role (domain + behavioral stance)
   - Workflow style recommendation

---

### A3: Propose Design (→ KR1, KR2)

Intent: Get approval on high-level design before investing in full draft
KR: Design approved by developer
Preconditions:
- Required: Intent documented from A2
Postconditions:
- Success: Output approved design with KRs and action outline
- Failure: Output design rejected, capture feedback for revision
Exit Conditions:
- Design rejected 3 times → stop, escalate to understand fundamental misalignment

Instructions:
1. Propose high-level design:
   - Goal, Intent, Scope, Role — one sentence each
   - Proposed KRs (3-4 measurable outcomes)
   - Proposed Actions — names and one-line purposes
   - Workflow style with rationale
   - Anticipated risks (high-level)
2. Ask for approval or feedback
3. If feedback given, revise and re-propose

---

### A4: Create Draft (→ KR2)

Intent: Generate complete skill following template structure
KR: Draft skill with all required sections
Preconditions:
- Required: Approved design from A3
- Required: skill-builder-template.md read
Postconditions:
- Success: Output complete skill draft
- Failure: Output partial draft with gaps identified
Exit Conditions:
- Template not accessible → stop, request template file

Instructions:
1. Read `.claude/workflows/skill-builder-template.md`
2. Fill template sections based on approved design:
   - Frontmatter (name, description, triggers, tools)
   - Goal, Intent, Scope, Role, Workflow Style
   - KRs from approved design
   - REQs based on project conventions
   - Pre/Postconditions
   - Steps following workflow style (hybrid)
   - Actions with complete structure
   - Notes section with skill-specific terms
   - Risks section (placeholder for A5)
   - References
3. Output draft for review

---

### A5: Analyze Risks (→ KR3)

Intent: Identify what could go wrong and propose mitigations
KR: Risks documented with mitigations
Preconditions:
- Required: Draft skill from A4
Postconditions:
- Success: Output risk analysis with severity and mitigations
- Failure: Output partial analysis with gaps noted
Exit Conditions:
- No risks identified → suspicious, probe harder for edge cases

Instructions:
1. Read `risk_management.md` for risk analysis approach
2. Analyze draft for risks in categories:
   - Scope risks — too broad? too narrow? overlapping?
   - Execution risks — what could fail? data loss? incorrect outputs?
   - Integration risks — conflicts with other skills or conventions?
   - Autonomy risks — too autonomous for impact? should confirm more?
   - Artifact risks — overwrites? clutter?
3. For each risk:
   - Assign severity (high/medium/low)
   - Propose mitigation
4. Output risk analysis
5. Ask developer to accept, modify, or add mitigations
6. Incorporate accepted mitigations into skill's Risks section

---

### A6: Iterate (→ KR2, KR4)

Intent: Refine skill based on developer feedback
KR: Skill matches developer's mental model
Preconditions:
- Required: Draft with risks from A4, A5
Postconditions:
- Success: Output refined skill ready for validation
- Failure: Output iteration blocked, capture unresolvable feedback
Exit Conditions:
- 3 iterations without convergence → stop, escalate to understand gap
- Developer approves → proceed to A7

Instructions:
1. Present draft skill to developer
2. Ask: "What would you change about this skill?"
3. If feedback:
   - Analyze requested changes
   - Apply changes to draft
   - Re-present for review
4. Track iteration count (max 3)
5. When approved, proceed to validation

---

### A7: Validate (→ KR4)

Intent: Verify skill meets quality standards with evidence
KR: Validation summary with positive and negative evidence
Preconditions:
- Required: Approved skill from A6
Postconditions:
- Success: Output validation passed with evidence, skill ready to write
- Failure: Output validation failures with remediation steps
Exit Conditions:
- Critical validation failure → stop, return to relevant action

Instructions:
1. Read `software_quality.md` and apply Quality Assessment
2. Check against validation checklist:
   - [ ] All sections present (Goal, Intent, Scope, Role, KRs, REQs, Pre/Post, Steps, Actions, Notes, Risks, References)
   - [ ] 3-4 Key Results, each measurable
   - [ ] 2+ Actions with complete structure
   - [ ] Each action has: Intent, KR, Preconditions, Postconditions, Exit Conditions, Instructions/Objectives
   - [ ] Coherent — no contradictions between sections
   - [ ] Complete — all KRs achievable from Actions
   - [ ] Referenced protocols exist
   - [ ] Follows CLAUDE.md conventions
3. For each criterion:
   - Document positive evidence (what confirms it passes)
   - Document negative evidence (what suggests it might fail)
4. Output validation summary
5. If all pass: write skill to `.claude/skills/{skill-name}/SKILL.md`
6. Update CLAUDE.md Available Skills table

---

## Additional Notes and Terms

**Skill Analysis Questions:**

*Understanding the Problem:*
1. What problem does this skill solve?
2. Why is this skill necessary? What happens without it?
3. Who will use this skill? (developer, CI, other skills)
4. What triggers this skill? (command, condition, other skill)

*Defining Scope:*
5. What are the deliverables?
6. Should this be one skill or multiple?
7. Does it compose with other skills?

*Determining Role:*
8. What domain role? (engineer, designer, analyst, reviewer)
9. What behavioral stance? (executor, advisor, pair programmer)

*Deciding Behavior:*
10. How autonomous should it be?
11. How should it handle errors?

**Workflow Style Guidance:**

| Style | When to Use |
|-------|-------------|
| Declarative | Goal-oriented, context varies widely, discovery needed |
| Imperative | Reproducibility critical, steps well-known, compliance required |
| Hybrid | Need consistency at boundaries, flexibility in middle |

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Skill doesn't follow conventions | Created without reading CLAUDE.md | A1 requires CLAUDE.md read as first step |
| 2 | Scope creep during creation | Intent expands through iteration | A2 documents scope explicitly; A6 checks changes against original scope |
| 3 | Over-complex skill | Too many actions for simple task | A3 design review catches over-engineering before drafting |
| 4 | Missing protocol references | Actions don't link to protocols | A7 validation checks protocol references exist |
| 5 | Skill overwrites existing | Name collision with existing skill | A1 checks existing skills; A7 validates no collision |

---

## References

- [skill-builder-template.md](../../workflows/skill-builder-template.md)
- [elicitation.md](../../protocols/elicitation.md)
- [software_quality.md](../../protocols/software_quality.md)
- [risk_management.md](../../protocols/risk_management.md)
- Project CLAUDE.md
