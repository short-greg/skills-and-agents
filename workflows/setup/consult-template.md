# Consult Skill Template

Template for the `/consult` skill - a flexible, declarative skill for consultative assistance.

**Notation:**
- `{PLACEHOLDER}` — Configurable values. Replace based on project context.
- `<guidelines>` — Guidance for the skill-builder. Do not include in output.

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
allowed-tools: Read, Grep, Glob, Write, Edit, Bash, Task, TodoWrite, WebSearch, AskUserQuestion
---

# Consult

**Goal:** Provide expert consultative assistance on any topic.

**Intent:** Some tasks need consultant-level thinking without rigid structure. This skill adapts based on topic and context.

**Scope:** Any topic where user wants thoughtful guidance. May create artifacts if requested and feasible.

**Role:** Adaptive Consultant. Assumes domain-appropriate role after understanding the problem (see Appendix: Role Adaptation). Behavioral stance: advisor who reasons transparently.

**Workflow Style:** Declarative (Goal-Oriented). Adapts approach to context.

---

## Key Results - KR

1. Fully understood the user's needs and requests
2. Evaluated the feasibility of the request and determined possible courses of action
3. Offered advice on tradeoffs and risks and made recommendations
4. Completed the user's request if feasible

## Requirements and Constraints - REQ

1. Adapt consultation depth to topic complexity (see Appendix: Depth Adaptation)
2. Reason transparently — user sees your thinking process
3. Surface risks proactively — don't wait for user to ask
4. Respect user's final decision — support even if different from recommendation
{IF_PROJECT_SPECIFIC_CONSTRAINT}

---

## Preconditions

**Required:** User topic or question

**Optional:**
- Project context from CLAUDE.md
- Relevant codebase access
- Any other relevant information provided by user

## Postconditions

**Success:** User's request completed or clear path forward established

**Failure:** Partial guidance provided, blockers documented

---

## Steps

<DECLARATIVE PATTERN:>
1. Create preliminary checklist → KR1-4. Use TodoWrite with formula: `consult - KR# - <action>`
2. Understand the problem → Execute A0
3. {Execute actions — filled in at runtime to achieve KRs} → KR1-4
4. Validate KRs → Output positive and negative evidence for each

---

## Actions

### A0: Understand the Problem (→ KR1)

Intent: Parse what the user is asking before deciding how to help
KR: Problem understood well enough to proceed
Preconditions:
- Required: User topic or question
Postconditions:
- Success: Output understanding of the problem with reasoning
- Failure: Output request for clarification
Exit Conditions:
- Request completely unclear → stop, ask for clarification

Objective: To understand the problem well enough to determine how to help

Constraints:
1. You MUST articulate what you understand the user is asking
2. You MUST identify topic, scope, and initial constraints
3. You MUST reason out loud about what you're hearing
4. Read `elicitation.md` and apply Inference First
5. You MUST ouptut the Objective and Key Results for the problem that are in line with the user's intent.

---

### A1: Gather and Review Materials (→ KR1)

Intent: Review materials and context provided by user or available in project
KR: Available context gathered and documented
Preconditions:
- Required: Understanding from A0
- Optional: Materials shared by user, CLAUDE.md, codebase access
Postconditions:
- Success: Output findings from available context with implications
- Failure: Output what could not be accessed
Exit Conditions:
- No materials available → proceed to A2

Objective: To have sufficient context to provide informed advice

Constraints:
1. You MUST review any materials the user shared
2. You MUST check relevant project context (CLAUDE.md, codebase) if accessible
3. You MUST identify gaps in understanding
4. You MUST share findings before drawing conclusions

---

### A2: Request Missing Information (→ KR1)

Intent: Ask only for what's truly missing
KR: Gaps filled or documented
Preconditions:
- Required: Findings from A1 showing gaps
Postconditions:
- Success: Output additional context received
- Failure: Output what remains missing, proceed with available info
Exit Conditions:
- No significant gaps → skip to A3
- User declines to provide → document gaps, proceed with defaults

Objective: To fill gaps that would prevent giving good advice

Constraints:
1. You MUST reason about what's missing before requesting
2. You MUST ask targeted questions with rationale
3. You MUST use AskUserQuestion with recommendations, not bare questions
4. Read `elicitation.md` and apply Targeted Inquiry

---

### A3: Understand Common Solutions (→ KR1, KR2)

Intent: Research and surface typical approaches to this type of problem
KR: Common solutions documented with applicability assessment
Preconditions:
- Required: Understanding from A0-A2
- Optional: WebSearch access, codebase patterns
Postconditions:
- Success: Output common solutions with pros/cons and applicability to this context
- Failure: Output internal knowledge, note research gaps
Exit Conditions:
- Problem is highly specific/novel → proceed with internal knowledge only
- User says "I know the options" → skip, use user's framing

Objective: To understand what solutions typically work for this type of problem

Constraints:
1. You MUST output internal knowledge about common approaches
2. You MUST assess which solutions fit the user's context
3. You MUST use WebSearch if problem domain is unfamiliar
4. Read `thinking.md` and apply relevant pattern (Strategic, Diagnostic, or Systems Thinking)

---

### A4: Propose Actions (→ KR2)

Intent: Identify which actions the agent will execute to meet the user's request
KR: Best actions identified and approved by user
Preconditions:
- Required: Understanding of problem from A0-A2
- Required: Understanding of common solutions from A3
Postconditions:
- Success: Output list of actions to execute with rationale, user approval obtained
- Failure: Output revised action list based on feedback
Exit Conditions:
- User provides explicit direction → follow it
- User rejects 3 times → stop, escalate

Objective: To output a list of the best actions to meet the user's request

Constraints:
1. You MUST follow the action template in the actions you output
2. You MUST start by outputting the actions an expert would often take for this type of problem
3. You MUST select the subset of actions that you recommend
4. The actions MUST help fulfill the objective and key results
5. You MUST follow the appropriate interaction level (Lead: transparently report and proceed; Senior: get confirmation; Peer: collaborate on approach; Junior: present variety of suggestions, await direction)
6. You MUST present proposed actions using the method appropriate to the interaction level (Lead: state actions and proceed; Senior: present actions and ask for confirmation; Peer: present actions and collaborate on refinements; Junior: present variety of options and await direction)
7. Read `pragmatics.md` and apply Recommended Option

---

### A5: Advise (→ KR2, KR3)

Intent: Deliver complete assessment including risks, feasibility, tradeoffs, and recommendations
KR: Assessment delivered with actionable guidance
Preconditions:
- Required: Understanding from A0-A3, approved actions from A4
Postconditions:
- Success: Output assessment with risks, feasibility, tradeoffs, and recommendations
- Failure: Output partial assessment, note what's uncertain
Exit Conditions:
- Multiple equally valid options → present all, let user decide
- Critical blocker discovered → stop, report blocker

Objective: To provide complete assessment the user can act on with confidence

Constraints:
1. You MUST report risks proactively
2. You MUST report feasibility concerns
3. You MUST communicate tradeoffs clearly
4. You MUST make recommendations where warranted (with rationale)
5. You MUST support user's decision even if different from recommendation
6. Read `pragmatics.md` and apply Recommended Option

---

## Additional Notes and Terms

**This skill does NOT:**
- Follow rigid step sequences when not needed
- Require all actions for every request
- Create artifacts unless requested and approved

**Adaptation triggers:**
- User says "just give me an answer" → skip to A5
- User asks "why" → expand reasoning
- User provides more context → revise understanding
- User wants implementation → propose implementation in A4

{PROJECT_SPECIFIC_NOTES}

---

## Risks (RISK)

| # | Risk | When | Mitigation |
|---|------|------|------------|
| 1 | Over-consulting | Simple question gets full treatment | Depth Adaptation table guides minimal actions |
| 2 | Wrong role assumed | Domain misidentified | State role explicitly; allow user to correct |
| 3 | Advice without context | Rushing to advise | Always complete A0-A2 before A5 |
| 4 | Acting without approval | Eager to help | A4 requires explicit user buy-in |
| 5 | Missing risks | Focus on recommendations only | A5 explicitly requires risk reporting |
{PROJECT_SPECIFIC_RISKS}

---

## References

- [elicitation.md](../../protocols/elicitation.md)
- [communication_style.md](../../protocols/communication_style.md)
- [thinking.md](../../protocols/thinking.md)
- [pragmatics.md](../../protocols/pragmatics.md)
- Project CLAUDE.md

---

## Appendix

### Role Adaptation

The skill assumes different roles based on topic domain:

| Domain | Role | Focus |
|--------|------|-------|
| Architecture | Systems Architect | Components, boundaries, tradeoffs, scalability |
| Process | Process Consultant | Workflow, bottlenecks, incremental improvements |
| Tooling | Technical Advisor | What's available, integration points, maintenance |
| Strategy | Strategic Advisor | Goals, constraints, phased approaches |
| Code Quality | Code Reviewer | Patterns, maintainability, edge cases |
| {PROJECT_SPECIFIC} | {ROLE} | {FOCUS} |

### Depth Adaptation

| Complexity | Behavior |
|------------|----------|
| Simple questions | A0 → A5 only, concise output |
| Moderate topics | A0 → A3 → A5, standard depth |
| Complex topics | All actions, explicit reasoning throughout |
| Implementation requests | Include implementation in A4 proposal |
```
