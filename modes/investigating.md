---
name: investigating
description: >
  Researching the problem or solutions. Gathers information through systematic investigation to reduce uncertainty before proceeding.
  You MUST satisfy the Goal and Key Results of this mode.
  Triggers on: "investigate", "research", "look into", "find out", "why is this happening",
  "what's the best practice", "check the docs", "find prior art", "what solutions exist".
  keywords: framing, searching, tracing, diagnosing, documenting, recommending
---

# Investigating

**Goal:** Reduce uncertainty by gathering and synthesizing information to enable informed decisions.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps through systematic research.

**Scope:** Gathering and synthesizing information to answer questions and reduce uncertainty. Answers "what do we need to know?" not "what should we build?" (defining) or "what steps?" (planning).

---

## Key Results - KR

1. Uncertainty reduced with grounded findings — sources cited, confidence stated, causes identified if debugging
2. Actionable recommendations provided — clear next steps based on investigation results

---

## Preconditions

**Required:** Question or topic to investigate

**Optional:**
- Context (related code, docs, logs, error messages)
- Investigation scope (what specifically needs to be known)
- Initial hypotheses, reproduction steps (if investigating bug)

## Postconditions

**Success:** Uncertainty mitigated with evidence, findings grounded in sources, confidence stated, recommendations provided

**Failure:** Insufficient access to information, scope too broad, question cannot be answered. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Frame Investigation | KR1 | To identify what's unknown and formulate hypotheses | Use when starting investigation or scope unclear | Read `protocols/thinking.md` and `protocols/risk_management.md` and apply appropriate techniques to assess uncertainty, risk, and complexity, then formulate testable hypotheses about causes or solutions | In: Question or topic (required), Known context (optional) | Out: Investigation scope with hypotheses |
| Search and Survey | KR1 | To gather evidence breadth-first across sources | Use when need landscape view or surveying available solutions | Read `protocols/discipline.md` and apply appropriate techniques to systematically search documentation, web, codebase, and logs, documenting findings with source citations | In: Search targets (required), Known sources (optional) | Out: Landscape survey with sources cited |
| Trace and Analyze | KR1 | To investigate depth-first and test hypotheses | Use when need to follow specific paths or verify explanations | Read `protocols/thinking.md` and `protocols/discipline.md` and apply appropriate techniques to trace execution paths, data flows, or dependency chains, analyze patterns, and test hypotheses systematically | In: Starting point or hypothesis (required), Evidence gathered (optional) | Out: Path documentation with hypothesis test results |
| Diagnose Causes | KR1 | To identify root causes of problems | Use when investigating bugs, failures, or unexpected behavior | Read `protocols/thinking.md` and apply appropriate techniques to distinguish causes from symptoms, identify multiple contributing factors, and verify diagnosis explains all observations | In: Symptoms observed (required), Test results (optional) | Out: Causes identified with supporting evidence |
| Document Findings | KR1 | To record evidence with sources and confidence levels | Use when investigation complete and evidence needs recording | Read `protocols/thinking.md` and `protocols/pragmatics.md` and apply appropriate techniques to document all findings, cite sources, and assess confidence level (high/medium/low) for each finding | In: All findings (required), Sources (required) | Out: Documented findings with sources and confidence levels |
| Recommend Actions | KR2 | To provide next steps based on findings | Use when findings documented and ready to advise | Read `protocols/pragmatics.md` and apply appropriate techniques to synthesize findings into actionable recommendations, prioritize by impact and confidence, and suggest next steps | In: Documented findings (required), Context (optional) | Out: Prioritized recommendations with rationale |

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [TapRoot - Six Root Cause Analysis Best Practices](https://taproot.com/root-cause-analysis-best-practices/)
- [TestDevLab - Core Principles of Root Cause Analysis](https://www.testdevlab.com/blog/what-is-root-cause-analysis)
