# Primitive Execution Protocol

**Type:** Protocol

Standard execution pattern for all primitives.

---

## Execution Steps

All primitives follow these steps:

1. **Track progress** using protocol `tracking.md`
2. **Create preliminary checklist** with key results using protocol `checklists.md`
3. **Resolve precondition uncertainties** — elicit missing required inputs
4. **Output action plan** using protocol `checklists.md` — which actions, what order
5. **Execute actions** — perform primitive-specific actions (see Actions section)
6. **Validate key results** using protocol `validation.md` — verify each KR, report success/failure

---

## Usage in Primitives

Each primitive references this in its Steps section:

```markdown
## Steps

Execute per protocol `primitive_execution.md` — track, plan, execute, validate.
```
