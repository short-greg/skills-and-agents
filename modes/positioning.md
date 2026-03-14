---
name: positioning
description: >
  Configuring yourself for task and user. Assumes role, adopts style, and calibrates approach before engaging.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "position yourself", "assume the role of", "adapt to the user", "configure for this task".
  keywords: assuming, adopting, calibrating, grounding, adjusting
---

# Positioning

**Goal:** Configure identity, communication style, and approach to align with task requirements and user context.

**Intent:** Without explicit positioning, AI defaults may not fit task or user. Metacognition research shows agents perform better when they configure themselves appropriately before engaging.

**Scope:** Internal self-configuration through assuming role, adopting style, and calibrating approach. Answers "how should I configure myself?" not "what should I do?" (planning) or "what's the situation?" (orienting).

---

## Key Results - KR

1. Position configured appropriately — role assumed, style adopted, calibration set for task and user
2. Position maintained or adjusted — configuration persists or explicitly shifts when context changes

---

## Preconditions

**Required:** Task or user context

**Optional:** User preferences, prior interaction history, explicit positioning request

## Postconditions

**Success:** Role assumed, style adopted, calibration set, position applied

**Failure:** Insufficient context. Output request for needed information (task, user, preferred style).

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Assess Context | KR1 | To read signals and determine positioning needs | Use when starting engagement or context has changed | Read and apply appropriate techniques from `protocols/elicitation.md` and `protocols/thinking.md` to analyze task domain, complexity, stakes, and infer user preferences from codebase, documentation, and interaction patterns | In: Task description (required), User context (optional) | Out: Context analysis with positioning requirements |
| Assume Role and Ground | KR1 | To take on appropriate role and anchor in domain | Use when context assessed and role not yet assumed | Read and apply appropriate techniques from `protocols/identity_and_profile.md` and `protocols/thinking.md` to select role (expert, mentor, peer, assistant), expertise level, perspective, and ground in domain knowledge and terminology | In: Context analysis (required), User expectations (optional) | Out: Role and domain grounding |
| Adopt Style and Calibrate | KR1 | To set communication parameters for task and user | Use when role assumed and style not yet set | Read and apply appropriate techniques from `protocols/communication_style.md` and `protocols/pragmatics.md` to set directness, formality, tone, verbosity, proactivity level, and technical complexity | In: Context analysis (required), Role (required), User preferences (optional) | Out: Style configuration |
| Apply Position | KR2 | To document or silently apply the position | Use when position configured and ready to engage | Read and apply appropriate techniques from `protocols/transparency.md` and `protocols/pragmatics.md` to document explicitly (when spawning sub-agents or building trust) or apply silently (when obvious from context) | In: Full position configuration (required) | Out: Position statement or silent application |
| Monitor, Adjust, or Escalate | KR1, KR2 | To check alignment and shift when needed | Use when engaged and feedback received or context shifts | Read and apply appropriate techniques from `protocols/thinking.md` to detect misalignment (user friction, repetitive failures), adjust position, or escalate to user when self-correction insufficient | In: Current position (required), User feedback or context signals (required) | Out: Adjustment or escalation with reasoning |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [AI Agents: Metacognition for Self-Aware Intelligence - Microsoft](https://techcommunity.microsoft.com/blog/educatordeveloperblog/ai-agents-metacognition-for-self-aware-intelligence---part-9/4402253)
- [Agentic Metacognition: Designing Self-Aware AI - arXiv](https://arxiv.org/abs/2509.19783)
- [Learning User Preferences Through Interaction - arXiv](https://arxiv.org/html/2601.02702v1)
- [Understanding Communication Preferences - arXiv](https://arxiv.org/html/2410.20468v2)
