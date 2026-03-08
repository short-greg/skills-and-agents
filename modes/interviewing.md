---
name: interviewing
description: >
  Gathering information from users through effective questioning. Elicits requirements, clarifies intent, and discovers constraints to reduce uncertainty.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "ask the user", "clarify requirements", "what do they want", "gather requirements",
  "interview the user", "find out what they need", "elicit requirements".
  keywords: asking, clarifying, eliciting, gathering, questioning
---

# Interviewing

**Goal:** Gather sufficient information to proceed confidently while minimizing user burden.

**Intent:** Without effective questioning, work proceeds on assumptions that may be wrong. Too many questions exhaust users; too few lead to rework. The cost of clarifying is always less than fixing bugs from wrong assumptions.

**Scope:** Eliciting information from users through questioning and inference. Answers "what does the user need?" not "what should we build?" Interviewing gathers input; defining specifies requirements; designing determines approach.

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

1. Information gathered — critical unknowns resolved, constraints discovered, success criteria understood
2. Understanding confirmed — user has validated interpretation, assumptions documented

## Requirements and Constraints - REQ

1. Infer before asking — check discoverable sources first
2. Batch questions appropriately — 1-2 at a time, prioritize critical ones
3. Document assumptions when proceeding without confirmation

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Terms

**Interviewing Target:** What information is being gathered (requirements, constraints, preferences, success criteria, context, etc.)

**Interviewing Method:** How to gather information (open questions, closed questions, probing, inference, constraint discovery, etc.)

---

## Preconditions

**Required:** Topic or question to clarify

**Optional:**
- Context from prior conversation
- Existing requirements or specifications
- Codebase or documentation to infer from
- Information request from failed mode (what's missing and what's needed)

## Postconditions

**Success:** Critical information gathered, understanding confirmed with user, assumptions documented

**Failure:** User cannot provide needed information, topic outside user's knowledge. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Parse Information Request (→ KR1)

**Goal:** Understand what information another mode needs

**When:** Received an information request from a failed mode

**Protocol:** `protocols/elicitation.md`

**Instructions:** Parse the request to identify what information is missing and what's needed to succeed. Determine what questions need to be asked to fulfill the request. Prioritize critical information. Use inference first to check if any requested information is already available.

**Inputs:**
- Information request from failed mode (required)
- Context from prior work (optional)

**Default Output:** Clarified understanding of what information is needed and prioritized question list

---

### Infer from Context (→ KR1)

**Goal:** Discover information without asking the user

**When:** Information may be discoverable from codebase, docs, or context

**Protocol:** `protocols/elicitation.md`

**Instructions:** Analyze codebase for patterns, check existing documentation, examine similar implementations, infer from context before asking. Use Inference First technique. Document what was discovered.

**Inputs:**
- Topic to clarify (required)
- Codebase or docs to search (optional)

**Default Output:** Discovered information with sources

---

### Ask Open Questions (→ KR1)

**Goal:** Explore unknown territory and understand context

**When:** Exploring new territory or understanding user's perspective

**Protocol:** `protocols/elicitation.md`

**Instructions:** Ask what/how/why questions, avoid yes/no framing, leave room for unexpected information. Use Open Questions and Funnel Sequencing techniques. Start broad, then narrow.

**Inputs:**
- Topic to explore (required)
- Prior context (optional)

**Default Output:** Questions for user

---

### Discover Constraints (→ KR1)

**Goal:** Surface hidden constraints before they cause rejection

**When:** Requirements may have unstated limits

**Protocol:** `protocols/elicitation.md`

**Instructions:** Ask about performance, compatibility, style, timeline. Probe for "obvious" expectations. Check for existing patterns to follow. Use Constraint Discovery technique.

**Inputs:**
- Proposed approach or requirement (required)
- Known constraints (optional)

**Default Output:** Discovered constraints

---

### Define Success Criteria (→ KR1)

**Goal:** Establish clear end state before starting

**When:** Scope or completion criteria unclear

**Protocol:** `protocols/elicitation.md`

**Instructions:** Ask "What would success look like?", probe for must-haves vs nice-to-haves, identify how they'll evaluate completion. Use Success Definition technique.

**Inputs:**
- Task or feature (required)
- Known requirements (optional)

**Default Output:** Success criteria

---

### Resolve Contradictions (→ KR1, KR2)

**Goal:** Surface and resolve conflicting information

**When:** User has given contradictory requirements

**Protocol:** `protocols/elicitation.md`

**Instructions:** Note contradiction without blame, present both statements neutrally, ask which takes priority, document resolution. Use Contradiction Resolution technique.

**Inputs:**
- Conflicting statements (required)

**Default Output:** Clarification request with resolution

---

### Confirm Understanding (→ KR2)

**Goal:** Verify interpretation before acting

**When:** Complex requirements have been gathered

**Protocol:** `protocols/elicitation.md`

**Instructions:** Paraphrase understanding back, summarize key points, ask for confirmation or correction, highlight any assumptions. Use Understanding Validation technique.

**Inputs:**
- Gathered information (required)
- Assumptions made (optional)

**Default Output:** Understanding summary for confirmation

---

### Document Assumptions (→ KR2)

**Goal:** Make implicit assumptions explicit and reviewable

**When:** Must proceed without confirmation

**Protocol:** `protocols/elicitation.md`

**Instructions:** State assumption clearly, explain basis for assumption, flag for user review, design for easy correction if wrong. Use Assumption Documentation technique.

**Inputs:**
- Assumptions being made (required)
- Basis for each assumption (required)

**Default Output:** Documented assumptions

---

## Additional Notes and Terms

**Interviewing vs Investigating:** Interviewing gathers information from users. Investigating gathers information from sources (code, docs, web). Use interviewing when the user has context that can't be discovered elsewhere.

**Interviewing vs Defining:** Interviewing elicits raw information. Defining structures that information into requirements with acceptance criteria. Interviewing often precedes or occurs within defining.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Requirements Elicitation Questions - Bridging the Gap](https://www.bridging-the-gap.com/what-questions-do-i-ask-during-requirements-elicitation/)
- [The Funnel Technique - NN/g](https://www.nngroup.com/articles/the-funnel-technique-in-qualitative-user-research/)
- [Art of Asking Clarifying Questions - AlgoCademy](https://algocademy.com/blog/the-art-of-asking-clarifying-questions-a-key-skill-for-programmers/)
