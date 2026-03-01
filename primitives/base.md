# Primitive Base

All primitives inherit this execution pattern.

---

## Execution Pattern

1. **Output lightweight checklist** per `tracking_and_recovery.md` — derive from Key Results, display inline
2. **Resolve preconditions** — elicit missing required inputs from user
3. **Plan actions** — select which actions apply to context, output execution order
4. **Execute actions** — placeholder to include primitive-specific actions per Actions section. Replace with actual actions.
5. **Confirm and report result** — verify each KR met, output success or failure

---

## Output Requirements

**Must report result** — Every primitive ends with explicit success or failure. Never complete silently.

**Inline tracking** — Output checklist and progress in conversation, not files. File-based tracking is for workflows.

**Lightweight** — Minimal overhead. Focus on doing the work, not documenting the work.

---

## Checklist Format

Output at start, update as work progresses:

```
## Progress
- [ ] KR1: [key result description]
- [ ] KR2: [key result description]
```

Mark complete as each is achieved:

```
## Progress
- [x] KR1: [key result description]
- [ ] KR2: [key result description]
```

---

## Completion Output

Always end with explicit result:

**Success:**
```
## Result: Success
- KR1: [evidence of completion]
- KR2: [evidence of completion]
```

**Failure:**
```
## Result: Failure
- KR1: [status - met/not met]
- KR2: [status - met/not met]
- Reason: [why primitive could not complete]
```
