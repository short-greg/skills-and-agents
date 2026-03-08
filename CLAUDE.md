# Skills and Agents Framework

Framework for building reliable AI coding assistant skills.

## Structure

- `modes/` — Internal cognitive actions used by workflows
- `workflows/` — User-invocable skills that compose modes
- `protocols/` — Reusable patterns referenced by modes/workflows
- `guidelines/` — Creation guides (consultative, not prescriptive)

Each directory has a README.md with details.

## Key Rules

**Workflows must:**
- Include progress tracking (prevents skipping steps)
- Reference modes for each task
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
