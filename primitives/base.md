# Primitive Base

All primitives must execute these steps.

---

## Steps

1. **Output lightweight checklist** per `tracking_and_recovery.md` — output lightweight checklist using Key Results, display inline
2. **Resolve preconditions** — elicit missing required inputs from user
3. **Evaluate actions** — output a table of actions including alternative actions then choose them based on the conditions and evaluate their usage in this situation | Action | Why Use | Why Not
4. **Output expert reasonined action plan** - Output the ideal type of expert for this problem and how they would tackle using `frame_of_mind.md` and how they would tackle this problem then decide the action plan using `planning.md` based on the action evaluation in step 3. 
5. [PLACEHOLDER: **Execute actions** — include primitive-specific actions per Actions section. Replace with actual actions. Must update with actions.]
6. **Confirm and report result** — verify each KR met, output success or failure

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
