# Skills and Agents Framework

Framework for building reliable AI coding assistant skills.

## Structure

- `workflows/` — User-invocable skills that compose actions
- `protocols/` — Reusable patterns referenced by actions
- `guidelines/` — Creation guides (consultative, not prescriptive)

Each directory has a README.md with details.

## Key Rules

**Workflows must:**
- Include progress tracking (prevents skipping steps)
- Reference protocols for each action
- Define preconditions and postconditions

**Guidelines must:**
- Be consultative (discover needs via interview, don't prescribe)
- Be tool-agnostic (use `${TOOL_CONFIG}` not `.claude/`)

**When editing this repo:**
- Don't list every file in docs (gets stale fast)
- Don't assume target project structure (detect it)
- Don't write tool-specific instructions

## Using This Framework

See [README.md](README.md) for installation in other projects.

To test: `/setup_env_workflow` skill in `.claude/skills/`
