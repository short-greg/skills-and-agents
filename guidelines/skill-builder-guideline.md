# Skill Builder Guideline

## Objective

Create a project-specific skill builder that understands THIS project's conventions, modes, protocols, and developer intent — then generates skills that fit.

## Key Results

1. Project conventions, modes, and protocols are understood (not just listed)
2. Developer intent for the skill builder is captured and confirmed
3. Skill builder is tailored to this project's specific needs
4. Skill builder creates skills that match project patterns

## Deliverables

1. .claude/skills/skill-builder/SKILL.md
2. .claude/skills/skill-builder/skill-template.md
3. Any other skills the user requests to get started with

---

## Process (Gated Steps)

You MUST complete each phase and get confirmation before proceeding to the next.

### Phase 1: Understand the Project

**Goal:** Deeply understand project conventions before designing anything.

**You MUST read:**
1. CLAUDE.md — project conventions, Interaction Mode, existing patterns
2. ALL files in `modes/` — understand what cognitive actions are available
3. ALL files in `protocols/` — understand what patterns are available
4. `workflow-creation-guideline.md` — understand how workflows should be structured
5. Existing skills (if any) — understand current patterns
6. Current Claude official skill builder skill as well as skill requirements using WebSearch

**Output:** Summary of what you learned:
- Project conventions discovered
- Interaction Mode (Lead/Senior/Peer/Junior)
- Available modes (list with one-line purpose each)
- Available protocols (list with one-line purpose each)
- Naming conventions
- Skill placement location

**Gate:** Do not proceed until you have output this summary.

---

### Phase 2: Understand Developer Intent

**Goal:** Understand what the developer needs from the skill builder.

**Interview about setting up the skill-builder:**
1. First, output what you are uncertain about in order to achieve the key results
2. Decide on topics to interview the developer about.
3. Example Topics:
   1. Whether to prefer project-level skills, system-level skills, or decide case-by-case?
   2. What problems will be worked on 
   3. How involved do you want to be when creating skills? (autonomous vs collaborative)
   4. What workflow style do you prefer? (declarative/imperative, prescriptive/descriptive)
   5. Are there specific skills you need first?
   6. Whether to require, recommend or not require (recommend require) the skills read over the CLAUDE.md and/or other documentation when they start?
   7. Whether to require, recommend, or not require (recommend to require) the skills it outputs use tracking? 
   8. Whether to require, recommend or not require the skills it outputs always validate at the end (recommend validate)?
   9. Whether to prefer declarative or imperative skills or no preference (recommend to have this be an option when creating the skill)? Explain the

**Output:** Your understanding of developer intent:
- Problem being solved
- Types of skills needed
- Collaboration level expected
- Style preferences

**Gate:** Get developer confirmation that you understood correctly. Do not proceed until confirmed.

---

### Phase 3: Design the Skill Builder

**Goal:** Design a skill builder tailored to Phase 1 + Phase 2 findings.

**The skill builder you design MUST:**
1. Reference the modes and protocols you read in Phase 1
2. Match the Interaction Mode from CLAUDE.md
3. Match the workflow style preferences from Phase 2
4. Use project naming conventions
5. Place skills in the correct location

**Other things you MUST consider to add as instructions to the skill builder because they are necessary when creating skills**
The developer may want to 
1. add other reference files when the create skills
2. add scripts to the skills
3. include scripts when they create skills
4. when creating a skill, the skill builder MUST act as a consultant at the appropriate level

**Output:** Proposed skill builder design:
- Goal, Intent, Scope
- Key Results (3-4)
- Steps (referencing modes)
- Tasks (each referencing one mode)
- How it will interview about each skill's use case

**Gate:** Get developer approval of design. Do not proceed until approved.

---

### Phase 4: Implement the Skill Builder

**Goal:** Create the skill builder skill file.

**Follow:**
- `workflow-creation-guideline.md` structure
- Project naming conventions
- Reference actual modes/protocols from Phase 1

**Place:** In the location identified in Phase 1

---

### Phase 5: Validate the Skill Builder

**Goal:** Confirm the skill builder meets requirements.

**For each criterion, output positive evidence, negative evidence, and decision:**

- [ ] References modes/protocols that actually exist
- [ ] Matches Interaction Mode from CLAUDE.md
- [ ] Matches workflow style from Phase 2
- [ ] Each task references exactly one mode
- [ ] Uses action/output-oriented language
- [ ] Follows workflow-creation-guideline.md structure

---

### Phase 6: Create Skills Using the Skill Builder

**Goal:** Use the skill builder to create skills. Follow ALL steps in the skill builder.

When creating a skill, you MUST follow every step defined in the skill builder. Do not skip steps.

---

## Skill Builder Internal Process

The skill builder you create must define and enforce this process for every skill it creates:

1. **Read project context** — CLAUDE.md, existing skills, conventions
2. **Interview about intent** — What problem? Why this skill? Who uses it?
3. **Review applicable modes** — Output table of modes that could apply
4. **Confirm understanding** — Output summary, get developer confirmation
5. **Design the skill** — Tailored to what was learned
6. **Implement the skill** — Following project conventions
7. **Validate** — Evidence-based evaluation

**IMPORTANT:** Every skill creation MUST follow ALL of these steps. The skill builder must be written to enforce this — no shortcuts, no skipping to implementation.

## Required Emphases

Skills created by the skill builder MUST emphasize:
- **Maintenance** — Keep documentation up to date
- **Tracking** — Use TodoWrite or trace files
- **Validation** — Evidence-based evaluation against KRs
- **Intent capture** — Interview about use case before creating

---

## Interaction Mode Reference

Read from CLAUDE.md. Determines confirmation level:

| Mode | Behavior |
|------|----------|
| **Lead** | Create with minimal confirmation |
| **Senior** | Confirm major decisions, proceed on implementation |
| **Peer** | Collaborate on design, confirm approach |
| **Junior** | Present all decisions for approval |

---

## References

- `workflow-creation-guideline.md`
- `mode-creation-guideline.md`
- `templates/skill_template.md`
- Project CLAUDE.md
