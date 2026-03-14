---
name: orienting
description: >
  Understanding the current state of a system, project, or task. Understands current state before deciding what to do.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation",
  "take stock", "size this up", "gap analysis", "assess the situation".
  keywords: mapping, inventorying, locating, surveying, discovering
---

# Orienting

**Goal:** Produce accurate picture of current state so next steps can be decided with confidence.

**Intent:** Prevent acting on assumptions. Poor orientation leads to solving the wrong problem, missing real issues, or duplicating existing work.

**Scope:** Discovery and synthesis of current situation. Includes mapping structure, inventorying assets, locating information, and identifying gaps. Answers "where are we now?" not "is this good?" (evaluating) or "where should we go?" (planning).

---

## Key Results - KR

1. Current state described accurately — structure mapped, assets inventoried, gaps surfaced
2. Orientation is actionable — reader knows what decisions need to be made

---

## Preconditions

**Required:** Subject to orient on

**Optional:**
- Current state (read relevant files, code, docs, outputs)
- Prior work (look for existing assessments, specs, decisions)
- Target state (if known, enables gap identification), success criteria

## Postconditions

**Success:** Current state documented accurately, gaps and issues surfaced, orientation is actionable

**Failure:** Cannot access subject, subject too large or ill-defined. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Map Structure | KR1 | To understand layout or topology of orienting target | Use when need to understand how parts relate to whole | Read and apply appropriate techniques from `protocols/discipline.md` and `protocols/system_modularity.md` to systematically enumerate components, document hierarchy and connections, and create representation showing structure | In: Subject to orient on (required), Scope boundaries (optional) | Out: Structure diagram or description |
| Inventory Assets | KR1 | To catalog existing resources | Use when need to know what exists | Read and apply appropriate techniques from `protocols/discipline.md` to systematically list all assets, categorize by type/purpose, and note completeness status | In: Subject to orient on (required), Categories to track (optional) | Out: Asset inventory with categories and status |
| Locate Information | KR1 | To find specific information needed for orientation | Use when specific information is needed but location unknown | Read and apply appropriate techniques from `protocols/elicitation.md` to search sources and document where critical information resides | In: Information needed (required), Known locations (optional) | Out: Information found with locations |
| Identify Gaps and Issues | KR1 | To discover what's missing or broken | Use when target state is known or standards exist | Read and apply appropriate techniques from `protocols/thinking.md` and `protocols/discipline.md` to enumerate gaps, missing components, and inconsistencies | In: Current state (required), Target state or standards (required) | Out: Gap list with severity |
| Summarize State and Flag Decisions | KR1, KR2 | To produce actionable summary of current state | Use when orientation findings complete | Read and apply appropriate techniques from `protocols/pragmatics.md` to produce concise summary and identify what decisions need to be made | In: All findings (required), Audience context (optional) | Out: Summary with flagged decision points |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Farnam Street - The OODA Loop](https://fs.blog/ooda-loop/)
- [Corporate Finance Institute - OODA Loop Guide](https://corporatefinanceinstitute.com/resources/management/ooda-loop/)
