---
name: investigating
description: >
  Researching the problem or solutions. Gathers information through systematic investigation to reduce uncertainty before proceeding.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "investigate", "research", "look into", "find out", "why is this happening",
  "what's the best practice", "check the docs", "find prior art", "what solutions exist".
  keywords: searching, surveying, diagnosing, analyzing, synthesizing
argument-hint: "[question or topic to investigate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch, WebFetch
---

# Investigating

**Goal:** Reduce uncertainty by gathering and synthesizing information to enable informed decisions.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps through systematic research.

**Scope:** Gathering and synthesizing information to answer questions and reduce uncertainty. Covers researching documentation and best practices, surveying solutions and prior art, tracing code paths and data flows, forming and testing hypotheses, diagnosing root causes, and synthesizing findings with actionable recommendations. Investigation answers "what do we need to know?" not "what should we build?"

---

## Key Results - KR

1. Uncertainty reduced with grounded findings — sources cited, confidence stated, root cause identified if debugging
2. Recommendations provided — clear next steps based on investigation results

## Requirements and Constraints - REQ

1. Assess own uncertainty explicitly — identify what LLM does not know
2. Form hypotheses before testing (when debugging or exploring causes)
3. Cite sources — where information came from
4. Always output recommendations for next steps

---

## Steps

MUST read and follow steps in `base_primitive.md`

---

## Terms

**Investigation Target:** What is being investigated (problem, root cause, solution landscape, best practices, how something works, etc.)

**Investigation Method:** How to investigate (web search, documentation review, code tracing, hypothesis testing, comparison, reproduction, etc.)

---

## Preconditions

**Required:** Question or topic to investigate

**Optional:**
- Context (related code, docs, logs, error messages)
- Investigation scope (what specifically needs to be known)
- Prior investigations (has this been looked into before)
- Initial hypotheses, reproduction steps (if investigating bug), constraints

## Postconditions

**Success:** Uncertainty mitigated with evidence, findings grounded in sources, confidence stated, recommendations for next steps provided

**Failure:** Insufficient access to information, scope too broad, question cannot be answered with available resources. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this primitive.

Select or propose actions based on context. Each action shows which KR it serves.

### Assess Uncertainty (→ KR1)

**Goal:** Identify what is unknown or uncertain

**When:** Beginning investigation

**Protocols:** `protocols/thinking.md`, `protocols/elicitation.md`

**Instructions:** Explicitly identify what LLM does not know or is unsure about. Apply interviewing when uncertain about context, intent, or constraints only user can provide. Document what needs to be learned. Use analytical thinking and open questions.

**Inputs:**
- Question or topic (required)
- Known context (optional)

**Default Output:** List of uncertainties and knowledge gaps

---

### Search Information (→ KR1)

**Goal:** Gather information from relevant sources

**When:** Information is not available in current context

**Protocol:** `protocols/discipline.md`

**Instructions:** Systematically search ALL relevant sources: web for documentation and best practices, codebase for related code and patterns, logs for error messages, official documentation for APIs. Track what was searched to ensure comprehensive coverage. Document findings with sources.

**Inputs:**
- Search targets (required)
- Known sources (optional)

**Default Output:** Findings with sources cited

---

### Trace Paths (→ KR1)

**Goal:** Follow execution, data, or system paths

**When:** Investigating how something works or where something happens

**Protocol:** `protocols/discipline.md`

**Instructions:** Systematically follow ALL relevant paths: execution paths (function calls, method chains), data flows (inputs → processing → outputs), system interactions (API calls, database queries). Track coverage to ensure no paths missed. Document the path taken and key observations.

**Inputs:**
- Starting point (required)
- Path type (optional)

**Default Output:** Path documentation with key observations

---

### Form Hypotheses (→ KR1)

**Goal:** Generate possible explanations

**When:** Evidence exists but explanation is unclear

**Protocol:** `protocols/thinking.md`

**Instructions:** Generate possible explanations based on gathered evidence. For each hypothesis: state it clearly, identify confirming evidence, identify refuting evidence, estimate likelihood. Consider what would need to be true for each hypothesis. Use analytical and counterfactual thinking.

**Inputs:**
- Evidence gathered (required)
- Context (optional)

**Default Output:** Hypotheses with supporting/refuting evidence and likelihood

---

### Test Hypotheses (→ KR1)

**Goal:** Confirm or refute hypotheses

**When:** Hypotheses exist

**Protocol:** `protocols/thinking.md`

**Instructions:** Design checks to confirm or refute each hypothesis. Reproduce issue or conditions to test prediction. Compare differences (working vs broken, before vs after). Make controlled changes and observe results. Document which hypotheses are confirmed, refuted, or remain uncertain. Use analytical thinking.

**Inputs:**
- Hypotheses to test (required)
- Test environment (optional)

**Default Output:** Test results with hypothesis status (confirmed/refuted/uncertain)

---

### Diagnose Root Cause (→ KR1)

**Goal:** Identify the root cause of a problem

**When:** Investigating bugs, failures, or unexpected behavior

**Protocol:** `protocols/thinking.md`

**Instructions:** Distinguish root cause from symptoms. Work backwards from symptom to cause. Verify diagnosis explains all observed symptoms. Consider what would happen if root cause were fixed. Document root cause with evidence and reasoning. Use analytical and counterfactual thinking.

**Inputs:**
- Symptoms observed (required)
- Test results (optional)

**Default Output:** Root cause with evidence and reasoning

---

### Synthesize Findings and Recommend (→ KR1, KR2)

**Goal:** Summarize investigation and provide next steps

**When:** Investigation completes

**Protocols:** `protocols/thinking.md`, `protocols/pragmatics.md`, `protocols/transparency.md`

**Instructions:** Summarize findings with supporting evidence and sources, clearly document reasoning process. State confidence level clearly (known vs suspected vs unknown). If researching solutions, enumerate viable approaches with tradeoffs. If diagnosing, state root cause with evidence. Provide specific, prioritized recommendations for next steps. Use strategic thinking, confidence signaling, and clear documentation.

**Inputs:**
- All findings (required)
- Context (optional)

**Default Output:** Summary with confidence levels, reasoning, and prioritized recommendations

---

## Additional Notes and Terms

**Investigation vs Evaluation:** Investigation gathers new information to reduce uncertainty. Evaluation assesses existing work against criteria. If evaluation reveals knowledge gaps, investigation fills them.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [TapRoot - Six Root Cause Analysis Best Practices](https://taproot.com/root-cause-analysis-best-practices/)
- [TestDevLab - Core Principles of Root Cause Analysis](https://www.testdevlab.com/blog/what-is-root-cause-analysis)
- [Bugasura - Root Cause Analysis Guide](https://bugasura.io/blog/root-cause-analysis-for-bug-tracking/)
