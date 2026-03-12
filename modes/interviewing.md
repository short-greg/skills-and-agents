---
name: interviewing
description: >
  Gathering information from users through effective questioning. Elicits requirements, clarifies intent, and discovers constraints to reduce uncertainty.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "ask the user", "clarify requirements", "what do they want", "gather requirements",
  "interview the user", "find out what they need", "elicit requirements".
  keywords: asking, clarifying, eliciting, gathering, questioning
---

# Interviewing

**Goal:** Gather sufficient information to proceed confidently while minimizing user burden.

**Intent:** Without effective questioning, work proceeds on assumptions that may be wrong. Too many questions exhaust users; too few lead to rework. The cost of clarifying is always less than fixing bugs from wrong assumptions.

**Scope:** Eliciting information from users through questioning and inference. Answers "what does the user need?" not "what should we build?"

---

## Key Results - KR

1. Information gathered — critical unknowns resolved, constraints discovered, success criteria understood
2. Understanding confirmed — user has validated interpretation, assumptions documented

---

## Preconditions

**Required:** Topic or question to clarify

**Optional:**
- Context from prior conversation
- Existing requirements or specifications
- Codebase or documentation to infer from

## Postconditions

**Success:** Critical information gathered, understanding confirmed with user, assumptions documented

**Failure:** User cannot provide needed information, topic outside user's knowledge. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Infer from Context | KR1 | To discover information without asking the user | Use when information may be discoverable from codebase, docs, or conversation context | Read `protocols/elicitation.md` and apply appropriate techniques to analyze available sources before asking. Consider checking codebase patterns, documentation, and similar implementations | In: Topic to clarify (required), Codebase or docs (optional) | Out: Discovered information with sources |
| Prioritize Questions | KR1 | To identify critical questions and batch appropriately | Use when multiple questions needed or starting interview | Read `protocols/elicitation.md` and apply appropriate techniques to identify critical unknowns, prioritize by importance, and batch questions (1-2 at a time) | In: Information gaps (required), Context (optional) | Out: Prioritized question list |
| Elicit Information | KR1 | To gather requirements, constraints, and success criteria | Use when need to ask user for information | Read `protocols/elicitation.md` and apply appropriate techniques to ask open questions, discover constraints, and define success criteria | In: Prioritized questions (required), Prior context (optional) | Out: Questions for user |
| Follow Up and Clarify | KR1, KR2 | To probe deeper and resolve uncertainties | Use when initial answers need clarification or contradictions exist | Read `protocols/elicitation.md` and apply appropriate techniques to ask follow-up questions, probe for details, and resolve uncertainties or contradictions | In: Initial responses (required), Identified gaps or contradictions (optional) | Out: Follow-up questions or clarification requests |
| Confirm and Document | KR2 | To validate understanding and document assumptions | Use when information gathered and ready to proceed | Read `protocols/elicitation.md` and `protocols/pragmatics.md` and apply appropriate techniques to paraphrase understanding back, summarize key points, and document any assumptions being made | In: Gathered information (required), Assumptions (optional) | Out: Understanding summary for confirmation and documented assumptions |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Requirements Elicitation Questions - Bridging the Gap](https://www.bridging-the-gap.com/what-questions-do-i-ask-during-requirements-elicitation/)
- [The Funnel Technique - NN/g](https://www.nngroup.com/articles/the-funnel-technique-in-qualitative-user-research/)
