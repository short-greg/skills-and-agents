---
name: positioning
description: >
  Configuring yourself for task and user. Establishes identity, communication style, and approach before engaging.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "position yourself", "configure for this", "set yourself up", "adapt to the user".
  keywords: configuring, calibrating, adapting, tuning
---

# Positioning

**Goal:** Configure identity, communication style, and approach to align with task requirements and user context.

**Intent:** Without explicit positioning, AI defaults may not fit task or user context. Metacognition research shows AI agents perform better when they configure themselves appropriately—choosing role, expertise, communication style, and approach before engaging prevents delivering correct content in wrong tone or claiming inappropriate expertise.

**Scope:** Self-configuration for task and user through selecting identity (role, expertise, perspective, domain) and configuring communication style (directness, formality, verbosity, tone). Answers "how should I configure myself?" not "what should I do?" (planning) or "what's the situation?" (orienting).

---

## Table of Contents

- [Key Results](#key-results---kr) — Success criteria for this mode
- [Requirements](#requirements-and-constraints---req) — Rules and constraints to follow
- [Steps](#steps) — Reference to base mode execution
- [Terms](#terms) — Key vocabulary and definitions
- [Preconditions](#preconditions) — What's needed before starting
- [Postconditions](#postconditions) — What's delivered upon completion
- [Actions](#possible-actions) — Concrete steps to achieve results
- [Notes](#additional-notes-and-terms) — Additional context and details
- [References](#references) — External documentation and resources

## Key Results - KR

1. Position configured appropriately — identity, style, and approach align with task and user context
2. Position applied consistently — configuration is maintained or explicitly adjusted when context shifts

## Requirements and Constraints - REQ

1. Validate alignment periodically — positioning may need adjustment as work evolves
2. Consider both task requirements AND user preferences when configuring
3. Document positioning when transparency aids collaboration, apply silently when it doesn't

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Preconditions

**Required:** Task or user context

**Optional:** User preferences, task requirements, prior interaction history, explicit positioning request

## Postconditions

**Success:** Identity, style, and approach configured appropriately

**Failure:** Insufficient context. Output request for needed information (task, user, preferred style).

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Analyze Context (→ KR1)

**Protocols:** `protocols/thinking.md`, `protocols/elicitation.md`

**Instructions:** Analyze task domain, complexity, stakes using analytical thinking. Infer user preferences from codebase, documentation, patterns before asking. Use targeted questions only if context insufficient.

**Inputs:**
- Task description (required)
- User context, prior interaction history (optional)

**Default Output:** Context analysis with task requirements and user preferences

---

### Configure Identity (→ KR1)

**Protocols:** `protocols/identity_and_profile.md`, `protocols/thinking.md`

**Instructions:** Select role, expertise level, perspective, domain grounding using strategic thinking. Apply identity positioning techniques.

**Inputs:**
- Context analysis, task requirements (required)
- User expectations (optional)

**Default Output:** Identity configuration

---

### Configure Communication Style (→ KR1)

**Protocols:** `protocols/communication_style.md`, `protocols/thinking.md`, `protocols/elicitation.md`

**Instructions:** Set directness, formality, verbosity, tone, stance using strategic thinking. Infer user preferences from context.

**Inputs:**
- Context analysis (required)
- User preferences (optional)

**Default Output:** Communication style configuration

---

### Output Position (→ KR2)

**Protocols:** `protocols/transparency.md`, `protocols/pragmatics.md`

**Instructions:** Document identity and style when transparency aids collaboration or spawning subagents. Make visible when building trust, apply silently otherwise.

**Inputs:**
- Identity and style configuration, context (required)

**Default Output:** Position statement or silent application

---

### Validate Position (→ KR1, KR2)

**Protocols:** `protocols/thinking.md`, `protocols/identity_and_profile.md`, `protocols/criteria_setting.md`

**Instructions:** Assess alignment: does identity match task? is style serving user? are behaviors consistent? Define effectiveness criteria, evaluate, reconfigure if misalignment detected.

**Inputs:**
- Current positioning, task progress, user feedback (required)

**Default Output:** Validation assessment with adjustment if needed

---

## Additional Notes and Terms

**Dynamic Positioning:** Reposition as task evolves or preferences become clearer. Acknowledge transitions explicitly.

**Silent vs Explicit:** Apply silently when obvious from context; document explicitly when it aids collaboration.

**Distinction from Orienting:** Orienting is external (understands situation); positioning is internal (configures yourself).

---

## References

**Claude Code documentation:**
- [Anthropic Claude Documentation - Agent Configuration](https://docs.anthropic.com/)

**Metacognition and self-awareness:**
- [AI Agents: Metacognition for Self-Aware Intelligence - Microsoft](https://techcommunity.microsoft.com/blog/educatordeveloperblog/ai-agents-metacognition-for-self-aware-intelligence---part-9/4402253)
- [Agentic Metacognition: Designing Self-Aware AI - arXiv](https://arxiv.org/pdf/2509.19783)

**Context-aware adaptation:**
- [PersoPilot: Adaptive AI-Copilot - arXiv](https://arxiv.org/html/2602.04540v1)
- [Context Engineering for Personalization - OpenAI](https://developers.openai.com/cookbook/examples/agents_sdk/context_personalization/)

**Agent calibration:**
- [Modeling and Optimizing User Preferences - arXiv](https://arxiv.org/pdf/2505.21907)
