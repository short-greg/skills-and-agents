# Skill Builder Guideline

## Objective

To create an effective skill builder skill that will meet project requirements

## Key Results

1. Current best practices for skills are understood especially how the official skill builder skill functions.
2. A skill builder skill is created and is aligned to the workflow-creation-guidelines.
3. The team's needs and project needs are thoroughly understood.
4. Project structure is appropriately accounted for in creating the skill builder skill.
5. The skill builder must be adaptive to different approaches such as being more declarative or imperative.

## Background

A skill builder skill is a project-specific skill that creates other skills. Each project has different conventions, structures, and team preferences. Rather than prescribing a fixed approach, the skill builder must be designed to discover and adapt to these differences. It acts as a consultant - understanding the project and team before generating skills that fit.

## Tasks

1. **Research best practices** — Use WebSearch to understand current skill builder patterns and how Claude Code's official skill builder functions
2. **Output Expert reasoning** - Output the typical expert role that will tackle consulting on building such skills and how they would tackle the problem. Elaborate on which tasks are best to complete and how best to complete them.
3. **Read project documentation** — Review CLAUDE.md, README, and conventions docs to understand project-specific requirements
4. **Investigate project structure** — Analyze repo layout, naming patterns, existing tooling, implicit conventions in codebase
5. **Interview team** — Understand team preferences, pain points, workflow style preferences (declarative vs imperative)
6. **Design skill builder** — Based on findings, design skill builder that fits project needs and team preferences
7. **Implement skill builder skill** — Create the skill builder following workflow_creation_guideline.md
8. **Validate with team** — Confirm skill builder works for their use cases, iterate if needed

## Required Emphases for Skills

The skill builder skill MUST ensure all skills it creates emphasize:
- **Maintenance** — Skills always keep documentation up to date and in line with feedback
- **Tracking** — Updates are always tracked (via TodoWrite, trace files, or similar)
- **Validation** — Use evidence-based evaluation to confirm alignment with key results, conventions, functional and non-functional requirements
- **Understanding use case** — The skill builder MUST interview about each skill's use case and intent before creating it. Do NOT just follow the template.

## Considerations to Account for When Creating the Skill Builder Skill

The skill builder skill must take the following into account
- **Project setup and Codebase** — How the project is set up to be AI friendly. 
  - Where CLAUDE.md is for instance. 
  - Where system documentations are located
  - Where the venv is
  - and so on
- **Team workflow style** 
  — When the user creates a skill they may want to make a skill more imperative (process-oriented) or declarative (goal-oriented)
  - When the user creates a skill they may want to make the skill very prescriptive or more descriptive. 
    - If it is more descriptive they outline the general guidelines but the skill must be set up so that it will come up with specific tasks to execute. This can be done through first doing expert-research or expert-reasoning about the problem it faces. 
    - With more prescriptive skills, underlying tasks it performs in the workflow for the skill be decided by the user.
  - It must account for whether the developer working on the project wants more hands on involvement or less hands on involvement. 
- **Workflow creation guidelines and consultation orientedness**
  - The outputted skill builder must follow these and be validated by them
  - The skill must be declarative in form and consultation oriented
  - It must review the latest best practices for skill building
  - It must present recommendations/suggestions in a way that is easy to review and address
- **Naming and structure conventions**
  - How skills should be named in this project
  - Where skill files should be placed
  - Any project-specific file naming patterns (snake_case, kebab-case, etc.)
- **Modes and tasks**
  - Review all modes that might be applicable for the skill, list them up in a table
- **Action or Output-oriented**
  - Skills that the skill-builder outputs must be action/output-oriented, avoid verbs that do not require concrete actions or outputs (e.g. instead of "consider X or Y" use "output a table for comparison")
- **Installation process**
  - The skill builder must describe how to install skills according to Claude's guidelines
- **Expert-oriented**
  - The skills skill-builder creates had better use reflection in which it first outputs how an expert would tackle the problem
- **Supporting files and dependencies**
  - What modes does the skill need to reference? Are they available?
  - What protocols should the skill use? Are they in the project?
  - Should missing modes/protocols be copied from the framework or created?
- **Scripts and shell functions**
  - Does the skill require shell functions (e.g., worktree helpers)?
  - Are there setup scripts needed for the skill to work?
  - How should these be installed and where should they live?
- **Skill placement and file structure**
  - Where should the created skill file be placed?
  - What directory structure does the project use for skills?
  - Are there supporting files that need to be created alongside the skill?

## Output

The skill builder skill produces:
- A complete skill file ready for use, or a draft for team review (depending on team preference)
- Skills that follow the project's conventions discovered during creation
- Skills validated against workflow_creation_guideline.md

## Validation

The skill builder skill should validate skills it creates:
- Follows workflow_creation_guideline.md structure
- Accounts for project-specific conventions
- Matches team's preferred workflow style
- References appropriate modes and protocols

## References

- Research current Claude Code skill builder patterns
- workflow_creation_guideline.md
- mode_creation_guideline.md
- Project-specific documentation discovered during consultation
