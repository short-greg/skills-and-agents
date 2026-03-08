# Worktree Setup Guideline

## Objective

To determine if the project benefits from git worktrees for parallel development, and if so, set them up according to team preferences.

## Key Results

1. Team's parallel work needs are understood
2. Decision made on whether worktrees are appropriate for this project
3. If appropriate, worktrees are configured to match team's workflow preferences
4. Setup is validated and team knows how to use it

## Background

Git worktrees enable parallel development by allowing work on multiple branches in separate directories. Not every project needs them. The setup workflow should assess whether worktrees fit the team's workflow, and if so, configure them based on team preferences rather than imposing a fixed structure.

## Tasks

1. **Assess parallel work needs** — Interview team about how they currently work. Do they switch between tasks? Work on multiple features? Need to keep main clean?
2. **Present options** — If worktrees seem beneficial, explain what they enable and let team decide
3. **Discover preferences** — What files need to sync? Where should worktrees live? What naming conventions?
4. **Configure based on preferences** — Set up worktrees according to what the team chose
5. **Validate with team** — Ensure setup works for their use cases

## Considerations

The worktree setup must account for:

- **Whether worktrees are needed at all**
  - Does the team work on multiple tasks in parallel?
  - Do they currently stash/switch frequently?
  - Would they benefit from isolated working directories?
  - Or do they prefer traditional branch switching?

- **Directory structure preferences**
  - Where should worktrees live relative to main repo?
  - What naming convention for worktree directories?
  - Should it match branch naming or be different?

- **Environment sync needs**
  - What files need to be copied to new worktrees? (.env, IDE settings, AI tool config)
  - Should specs be shared across worktrees or isolated?
  - Are there project-specific files that must sync?

- **Shell integration preferences**
  - Do they want helper functions (git-worktree-add, etc.)?
  - Which shell do they use (bash, zsh, fish)?
  - Do they prefer aliases or full function names?

- **Integration with existing workflow**
  - How does this fit with their branching strategy?
  - Does their CI/CD need to account for worktrees?
  - How should completed worktrees be cleaned up?

## Orchestration Workflow Considerations

If the team wants coordinated parallel work (not just manual worktree usage), consider creating orchestration workflows:

- **Whether orchestration is needed**
  - Will they decompose large features into parallel tasks?
  - Do they need dependency tracking between tasks?
  - Or will they manually manage worktrees without coordination?

- **Worktree orchestrate workflow**
  - Breaks a plan into 2-6 independent tasks
  - Creates dependency graph (which tasks must complete first)
  - Identifies file overlap risks (tasks modifying same files)
  - Sets up worktrees for tasks ready to start
  - Merges completed tasks in dependency order
  - How should task decomposition work for this project? By component? By feature? By layer?

- **Worktree task workflow**
  - Executes a single task in a worktree
  - Checks dependencies are merged before starting (hard gate)
  - Routes to appropriate workflow (feature, bugfix, refactor)
  - Validates against acceptance criteria
  - Signals ready for merge
  - What validation is needed before signaling complete?

- **Task spec format**
  - How should task specs be structured?
  - What metadata is needed (dependencies, affected files, acceptance criteria)?
  - Where should task specs be stored?

- **Integration preferences**
  - How strict should dependency enforcement be?
  - Should code review be required before merge?
  - What happens on merge conflict or test failure?

## Output

If worktrees are appropriate:
- Worktree directory structure configured to team's preferences
- Shell functions installed (if wanted)
- .worktreesync file with team's chosen files
- Documentation of how the team decided to use worktrees

If orchestration workflows are needed:
- Worktree orchestrate workflow customized to team's decomposition preferences
- Worktree task workflow with appropriate routing and validation
- Task spec template matching team's needs

If worktrees/orchestration are not appropriate:
- Documentation of why the team decided against them
- Alternative approaches if they have parallel work needs

## Validation

- [ ] Team understands what worktrees enable
- [ ] Decision on worktrees reflects team's actual workflow
- [ ] If configured, setup matches team's stated preferences
- [ ] Team can successfully create and remove a worktree
- [ ] Environment files sync as expected

## References

- Git worktree documentation: https://git-scm.com/docs/git-worktree
- Shell function implementations available in old docs/SETUP_WORKTREE_ENV.md (commit d798b65)
