

[COMMENT: This is currently really bad. 
It is overly prescriptive and not taking a consultant approach.

I am trying to get away from that and you back toward it


Requirements

1. Skill builder interview, investigate the user to understand the needs of user and project then create the appropriate
2. Skill builder must check out current
3. Skill builder must recommend certain actions such as always checking CLAUDE.md

This 

]

## Objective

To create an effective skill builder skill that will meet project requirements

## Key Results

1. Current best practices for skills are understood especially how the official skill builder skill functions.
2. A skill builder skill is created and is aligned to the workflow-creation-guidelines.
3. The team's needs and project needs are thoroughly understood.
4. Project structure is appropriately accounted for in creating the skill builder skill.
5. The skill builder must be adaptive to different approaches such as being more declarative or imperative.

## Background

[COMMENT: Add brief rationale]

## Tasks

1. **Research best practices** — Use WebSearch to understand current skill builder patterns and how Claude Code's official skill builder functions
2. **Output Expert reasoning** - Output the typical expert role that will tackle consulting on building such skills and how they would tackle the problem. Elaborate on which tasks are best to complete and how best to complete them.
3. **Read project documentation** — Review CLAUDE.md, README, and conventions docs to understand project-specific requirements
4. **Investigate project structure** — Analyze repo layout, naming patterns, existing tooling, implicit conventions in codebase
5. **Interview team** — Understand team preferences, pain points, workflow style preferences (declarative vs imperative)
6. **Design skill builder** — Based on findings, design skill builder that fits project needs and team preferences
7. **Implement skill builder skill** — Create the skill builder following workflow_creation_guideline.md
8. **Validate with team** — Confirm skill builder works for their use cases, iterate if needed

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
- **Workflow creation guidelines and consultation Orientedness**
  - The outputted skill builder must follow these and be validated by them
  - The skill must be declarative in form and consultation oriented.
  - It must review the latest best practices for skill building.

  - It must also present recommendations/suggestions in a way so they are easy to review and address.
