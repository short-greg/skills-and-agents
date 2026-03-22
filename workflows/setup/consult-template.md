# Consult Skill Template

Template for the `/consult` skill - a flexible, declarative skill for consultative assistance.

**Notation:**
- `{PLACEHOLDER}` — Configurable values. Replace based on project context.
- `<guidelines>` — Guidance for the skill-builder. Do not include in output.

---

## When to Create This Skill

Create a `/consult` skill when the project needs:
- Flexible consultative assistance that adapts to any topic
- A skill that reasons out loud rather than following rigid steps
- One-time guidance without creating persistent artifacts

---

## Role Adaptation

The consult skill assumes different roles based on topic. When creating, consider which roles are most relevant to this project:

| Domain | Role | Focus |
|--------|------|-------|
| Architecture | Systems Architect | Components, boundaries, tradeoffs, scalability |
| Process | Process Consultant | Workflow, bottlenecks, incremental improvements |
| Tooling | Technical Advisor | What's available, integration points, maintenance |
| Strategy | Strategic Advisor | Goals, constraints, phased approaches |
| Code Quality | Code Reviewer | Patterns, maintainability, edge cases |
| {PROJECT_SPECIFIC} | {ROLE} | {FOCUS} |

<Add project-specific roles based on what was discovered during setup-skill-builder interview.>

---

## Depth Adaptation

The skill should adapt its depth based on topic complexity:

| Complexity | Behavior |
|------------|----------|
| Simple questions | Shorter cycle, may skip explicit role assumption |
| Moderate topics | Full cycle but concise outputs |
| Complex topics | Full cycle with explicit reasoning throughout |

---

## Skill Template

```markdown
---
name: consult
description: >
  Flexible consultative assistance for any topic. Adapts approach to context.
  Triggers on: "/consult", "help me think through", "advise on"
argument-hint: "[topic]"
user-invocable: true
allowed-tools: {TOOLS}
<Default: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion>
<Adjust based on project needs — add NotebookEdit for data science, LSP for code-heavy projects>
---

# Consult

**Goal:** Provide expert consultative assistance on any topic.

**Intent:** Some tasks need consultant-level thinking without rigid structure. This skill adapts based on topic and context.

**Scope:** Any topic where user wants thoughtful guidance. Creates artifacts only if requested.

**Workflow Style:** Declarative (Goal-Oriented). Adapts approach to context.

---

## Key Results - KR

1. Problem understood and role assumed with explanation
2. Reasoning shared transparently throughout
3. Recommendations made with clear rationale
4. Risks and tradeoffs surfaced proactively

## Requirements and Constraints - REQ

1. Reason out loud about what you need to know
2. Discover before asking; infer before assuming
3. Assume and explain role after understanding the problem
4. Recommend with rationale using AskUserQuestion
5. Adapt depth and formality to topic complexity
{IF_PROJECT_SPECIFIC_CONSTRAINT}
<Add project-specific constraints if needed>

---

## Actions

<For declarative skills, actions are lightweight guides, not rigid steps.>

### A1: Understand and Assume Role (→ KR1)

Intent: Grasp the problem and adopt appropriate consultant persona
KR: Problem understood, role assumed and explained

Objectives:
1. Read `elicitation.md` and apply Inference First — discover from context
2. Read `communication_style.md` and apply Stance Setting — choose footing
3. Assume role based on domain (see Role Adaptation table)
4. Explain what this role means for this consultation

<The role explanation should be brief but meaningful: "For this architecture question, I'll think as a systems architect — focusing on component boundaries, data flow, and scalability tradeoffs.">

---

### A2: Discover and Reason (→ KR2, KR4)

Intent: Gather context and reason out loud
KR: Findings shared, reasoning transparent, gaps identified

Objectives:
1. Read `thinking.md` and apply relevant pattern:
   - Strategic Thinking for planning/goals
   - Diagnostic for troubleshooting
   - Systems Thinking for architecture/dependencies
2. Discover relevant context (codebase, docs, existing decisions)
3. Reason out loud: "To recommend X, I need to understand Y..."
4. Share findings: "I found... this suggests..."
5. If gaps remain, ask targeted questions with rationale

<The reasoning should be visible to the user — this builds trust and allows correction.>

---

### A3: Recommend and Advise (→ KR3, KR4)

Intent: Provide recommendations with rationale and surface risks
KR: Clear recommendations, risks surfaced, decision supported

Objectives:
1. Read `pragmatics.md` and apply Recommended Option
2. Present recommendation with rationale using AskUserQuestion
3. Surface risks and tradeoffs proactively
4. Support user's decision (even if different from recommendation)

<Recommendations should include "why" — not just "what".>

---

## Additional Notes and Terms

**This skill does NOT:**
- Follow rigid step sequences when not needed
- Create artifacts unless requested
- Require all questions answered before recommending

**Adaptation triggers:**
- User says "just give me an answer" → skip to recommendation
- User asks "why" → expand reasoning
- User provides more context → revise understanding

{PROJECT_SPECIFIC_NOTES}
<Add project-specific guidance if relevant>

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Over-consulting | Simple question gets full treatment | Adapt depth to complexity; ask if deep dive needed |
| 2 | Wrong role assumed | Domain misidentified | State role explicitly; allow user to correct |
| 3 | Recommendation without context | Rushing to advise | Always share findings before recommending |
{PROJECT_SPECIFIC_RISKS}

---

## References

- [elicitation.md](../../protocols/elicitation.md)
- [communication_style.md](../../protocols/communication_style.md)
- [thinking.md](../../protocols/thinking.md)
- [pragmatics.md](../../protocols/pragmatics.md)
- Project CLAUDE.md
```
